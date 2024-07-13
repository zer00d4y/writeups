# PermX Write-up

<img src="https://labs.hackthebox.com/storage/avatars/3ec233f1bf70b096a66f8a452e7cd52f.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV -p- permx.htb`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 e2:5c:5d:8c:47:3e:d8:72:f7:b4:80:03:49:86:6d:ef (ECDSA)
    |_  256 1f:41:02:8e:6b:17:18:9c:a0:ac:54:23:e9:71:30:17 (ED25519)
    80/tcp open  http    Apache httpd 2.4.52
    |_http-server-header: Apache/2.4.52 (Ubuntu)
    |_http-title: eLEARNING
    Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Directory enumeration with Dirsearch

`python3 dirsearch.py -u http://permx.htb --exclude-status 403,404,400,401,503 -o dir`

    301 -  303B  - /js  ->  http://permx.htb/js/                     
    200 -   10KB - /404.html                                         
    200 -   20KB - /about.html                                       
    200 -   14KB - /contact.html                                     
    301 -  304B  - /css  ->  http://permx.htb/css/                   
    301 -  304B  - /img  ->  http://permx.htb/img/                   
    200 -  922B  - /js/                                              
    301 -  304B  - /lib  ->  http://permx.htb/lib/                   
    200 -    2KB - /lib/                                             
    200 -    1KB - /LICENSE.txt    

### Sub-Domain enumeration with FFuF

`ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt -u http://permx.htb -H "Host:FUZZ.permx.htb" -fw 18`

    
        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       


    lms                     [Status: 200, Size: 19347, Words: 4910, Lines: 353, Duration: 201ms]
    www                     [Status: 200, Size: 36182, Words: 12829, Lines: 587, Duration: 175ms]

Found lms.permx.htb ; www.permx.htb

Add domain name to `/etc/hosts`

    sudo echo "10.10.11.23 permx.htb lms.permx.htb " | sudo tee -a /etc/hosts

permx.htb

![image](https://github.com/user-attachments/assets/923a8e70-e12c-408a-99ec-5fbf8ab977ec)

lms.permx.htb

![image](https://github.com/user-attachments/assets/4b4d61d7-0ccb-46c8-a590-b0a31a2bc4c8)


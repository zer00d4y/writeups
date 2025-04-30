# Pickle Rick Write-up

<img src="https://tryhackme-images.s3.amazonaws.com/room-icons/47d2d3ade1795f81a155d0aca6e4da96.jpeg" width="200" height="200">

Difficulty: Easy

room link: https://tryhackme.com/room/picklerick

## Recon 

### Nmap 

`nmap -sC -sV 10.10.72.6`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 95:1c:a2:a3:77:50:47:4a:f9:4e:5d:98:24:b0:a6:e2 (RSA)
    |   256 8a:ea:cf:b0:44:af:6e:0a:ab:86:fc:1d:8e:7b:b7:20 (ECDSA)
    |_  256 d0:20:94:9a:68:72:8a:b0:f3:37:d5:a2:11:6c:8d:43 (ED25519)
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
    |_http-title: Rick is sup4r cool
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Dirsearch 

`python3 dirsearch.py -u http://10.10.72.6/ --exclude-status 403,404,400,401 -o dir`

    Scanning: 
    301 -   309B - /assets  ->  http://10.10.72.6/assets/            
    200 -    2KB - /assets/                                          
    200 -    1KB - /index.html                                       
    200 -   882B - /login.php                                        
    200 -    17B - /robots.txt      

/index.html main page

![image](https://github.com/user-attachments/assets/0b6475c3-1279-47e5-a3d2-f57919f5e4c8)

![image](https://github.com/user-attachments/assets/c34ef216-9015-484c-9cc9-f9f238ccb21c)

    Note to self, remember username!

    Username: R1ckRul3s

![image](https://github.com/user-attachments/assets/2fef318e-ec8e-4bb0-bdf7-9b6157608ef2)

    Wubbalubbadubdub



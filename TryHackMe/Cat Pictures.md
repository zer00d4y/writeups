# Cat Pictures Write-up

<img src="https://tryhackme-images.s3.amazonaws.com/room-icons/4c424fa649d64938ae8282b14e4299ac.png" width="200" height="200">

Difficulty: Easy

room link: https://tryhackme.com/room/catpictures

## Nmap
                                                                                                               
    ┌──(kali㉿kali)-[~]
    └─$ nmap -sV -sC 10.10.103.45     
    Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-02-04 23:55 EST
    Nmap scan report for 10.10.103.45
    Host is up (0.14s latency).
    Not shown: 998 closed tcp ports (conn-refused)
    PORT     STATE SERVICE VERSION
    22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 37:43:64:80:d3:5a:74:62:81:b7:80:6b:1a:23:d8:4a (RSA)
    |   256 53:c6:82:ef:d2:77:33:ef:c1:3d:9c:15:13:54:0e:b2 (ECDSA)
    |_  256 ba:97:c3:23:d4:f2:cc:08:2c:e1:2b:30:06:18:95:41 (ED25519)
    8080/tcp open  http    Apache httpd 2.4.46 ((Unix) OpenSSL/1.1.1d PHP/7.3.27)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 123.28 seconds

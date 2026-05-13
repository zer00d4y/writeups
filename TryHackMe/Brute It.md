# Brute It Write-up

Reconnaissance

NMap

`nmap -sV -sC 10.48.168.98`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
    |   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
    |_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (ED25519)
    80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    |_http-title: Apache2 Ubuntu Default Page: It works
    MAC Address: 02:6C:B6:74:D4:CB (Unknown)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Dirsearch

`dirsearch -u http://10.48.168.98`

      _|. _ _  _  _  _ _|_    v0.4.3.post1
     (_||| _) (/_(_|| (_| )
    
    Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25
    Wordlist size: 11460
    
    Target: http://10.48.168.98/
    
    Starting: 
    301 -  312B  - /admin  ->  http://10.48.168.98/admin/
    200 -  385B  - /admin/
    200 -  385B  - /admin/index.php

--------------------------------------------

Search for open ports using nmap.
How many ports are open?

    2

Search for open ports using nmap.
How many ports are open?

    OpenSSH 7.6p1

What version of Apache is running?

    2.4.29

Which Linux distribution is running?

    Ubuntu

Search for hidden directories on web server.
What is the hidden directory?

    /admin
--------------------------------------------

Getting a shell



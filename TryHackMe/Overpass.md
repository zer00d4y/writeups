# Overpass Write-up

<img src="https://github.com/zer00d4y/writeups/assets/128820441/cd549821-eac7-45d6-9b33-3e3d28b99a45" width="200" height="200">

Difficulty: Easy

room link: https://tryhackme.com/room/overpass

### Nmap

`nmap -sC -sV overpass.thm`

    ┌──(kali㉿kali)-[~]
    └─$ nmap -sC -sV overpass.thm
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
    |   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
    |_  256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
    80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
    |_http-title: Overpass
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

`nmap overpass.thm`

    ┌──(kali㉿kali)-[~/tools/dirsearch]
    └─$ nmap overpass.thm    

    PORT     STATE    SERVICE
    22/tcp   open     ssh
    80/tcp   open     http
    873/tcp  filtered rsync
    9002/tcp filtered dynamid


### Directory enumeration with Dirsearch

`python3 dirsearch.py -e php,html,js -u http://overpass.thm/ --exclude-status 404,403,401,307`

    ┌──(kali㉿kali)-[~/tools/dirsearch]
    └─$ python3 dirsearch.py -e php,html,js -u http://overpass.thm/ --exclude-status 404,403,401,307
    
      _|. _ _  _  _  _ _|_    v0.4.3
     (_||| _) (/_(_|| (_| )
    
    Starting:           
    200 -  782B  - /404.html                                                                    
    200 -    1KB - /admin.html                                       
    200 -    1KB - /admin/                                                               
    200 -    2KB - /downloads/                           
    200 -    2KB - /login.js                                                     
    200 -   28B  - /main.js                                          

Check /admin

![image](https://github.com/zer00d4y/writeups/assets/128820441/f5a45253-5965-4a0a-91b9-5dbef6e1faa8)

Look source of the page 

    <!DOCTYPE html>
    <html>
    
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title>Overpass</title>
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" type="text/css" media="screen" href="/css/main.css">
        <link rel="stylesheet" type="text/css" media="screen" href="/css/login.css">
        <link rel="icon" type="image/png" href="/img/overpass.png" />
        <script src="/main.js"></script>
        <script src="/login.js"></script>
        <script src="/cookie.js"></script>
    </head>
    
    <body onload="onLoad()">
        <nav>
            <img class="logo" src="/img/overpass.svg" alt="Overpass logo">
            <h2 class="navTitle"><a href="/">Overpass</a></h2>
            <a class="current" href="/aboutus">About Us</a>
            <a href="/downloads">Downloads</a>
        </nav>
        <div class="content">
            <h1>Administrator area</h1>
            <p>Please log in to access this content</p>
            <div>
                <h3 class="formTitle">Overpass administrator login</h1>
            </div>
            <form id="loginForm">
                <div class="formElem"><label for="username">Username:</label><input id="username" name="username" required></div>
                <div class="formElem"><label for="password">Password:</label><input id="password" name="password"
                        type="password" required></div>
                <button>Login</button>
            </form>
            <div id="loginStatus"></div>
        </div>
    </body>
    
    </html>

Open login.js, 


    ...
        if (statusOrCookie === "Incorrect credentials") {
            loginStatus.textContent = "Incorrect Credentials"
            passwordBox.value=""
        } else {
            Cookies.set("SessionToken",statusOrCookie)
            window.location = "/admin"
        }
    ...

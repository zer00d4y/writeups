# Mist Write-up

<img src="https://labs.hackthebox.com/storage/avatars/84669b838a8633d26f4a2d90a6069f7e.png" width="200" height="200">

## Nmap

`nmap -sC -sV -p- mist.htb`

    PORT   STATE SERVICE VERSION
    80/tcp open  http    Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
    |_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
    | http-title: Mist - Mist
    |_Requested resource was http://10.10.11.17/?file=mist
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    |_http-generator: pluck 4.7.18
    | http-robots.txt: 2 disallowed entries 
    |_/data/ /docs/




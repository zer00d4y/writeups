# IClean Write-up

<img src="https://labs.hackthebox.com/storage/avatars/750ba886c8a87103c69cac0f13f2de70.png" width="200" height="200">

## Nmap

`nmap -sC -sV 10.10.11.12`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 2c:f9:07:77:e3:f1:3a:36:db:f2:3b:94:e3:b7:cf:b2 (ECDSA)
    |_  256 4a:91:9f:f2:74:c0:41:81:52:4d:f1:ff:2d:01:78:6b (ED25519)
    80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
    |_http-server-header: Apache/2.4.52 (Ubuntu)
    |_http-title: Site doesn't have a title (text/html).
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## Dirsearch 

`python3 dirsearch.py -u http://capiclean.htb/ --exclude-status 403,404,400,401 -o dir`

    Target: http://capiclean.htb/
    
    Starting:                                                                                                                  
    200 -    5KB - /about                                            
    302 -  189B  - /dashboard  ->  /                                 
    200 -    2KB - /login                                            
    302 -  189B  - /logout  ->  /                                    
    200 -    8KB - /services                                         
                                                                                 
    Task Completed                                        

http://capiclean.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/82f197be-44fd-4dbc-a912-8be4e5a407cb)



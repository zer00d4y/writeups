# Usage Write-up

<img src="https://labs.hackthebox.com/storage/avatars/23e804513a47e8f20bc865d0419946e1.png" width="200" height="200">

## Nmap

`nmap -sC -sV -p- usage.htb`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
    |_  256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-title: Daily Blogs
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add `usage.htb` to `/etc/hosts`

## Dirsearch

`python3 dirsearch.py -u http://usage.htb/ --exclude-status 403,404,400,401,503 -o dir`

    Target: http://usage.htb/
    
    [07:05:38] Starting:                                                                                                                  
    [07:07:13] 302 -  334B  - /dashboard/  ->  http://usage.htb/login           
    [07:08:21] 200 -   24B  - /robots.txt                                       
                                                                                 
    Task Completed                                                                                                                        
                                         
http://usage.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/3483821e-2005-4105-aabf-409e5547619e)

Create accaount and login

![image](https://github.com/zer00d4y/writeups/assets/128820441/2c090840-9d55-4309-8c41-275ec38baf12)


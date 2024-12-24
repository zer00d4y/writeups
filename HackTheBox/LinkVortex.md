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
                                         

### Sub-Domain enumeration 



## Exploitation 

### CVE-2023-40028

CVE-2023-40028 is a vulnerability in Ghost CMS that allows arbitrary file reads through improper file handling. 

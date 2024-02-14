# Kitty Write-up

<img src="https://tryhackme-images.s3.amazonaws.com/room-icons/4fda99da293ec0bc477a4b9e456e55e1.png" width="200" height="200">

Difficulty: Medium

room link: https://tryhackme.com/room/kitty

### Port scaning

`nmap -sC -sV kitty.thm`
                                                                                                                        
    ┌──(root㉿kali)-[/home/kali]
    └─# nmap -sC -sV kitty.thm
    
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 b0:c5:69:e6:dd:6b:81:0c:da:32:be:41:e3:5b:97:87 (RSA)
    |   256 6c:65:ad:87:08:7a:3e:4c:7d:ea:3a:30:76:4d:04:16 (ECDSA)
    |_  256 2d:57:1d:56:f6:56:52:29:ea:aa:da:33:b2:77:2c:9c (ED25519)
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
    |_http-title: Login
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### SSH

`ssh kitty@kitty.thm`

password for ssh: `L0ng_Liv3_KittY`

    kitty@kitty:~$ ls
    user.txt
    kitty@kitty:~$ cat user.txt 
    THM{31e606998972c3c6baae67bab463b16a} 
                                                                                                                                        
### Privilege escalation


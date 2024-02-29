# Devvortex Write-up

<img src="https://labs.hackthebox.com/storage/avatars/2565d292772abc4a2d774117cf4d36ff.png" width="200" height="200">

---------------------------------------------------------------------------------------------------------------------

# Nmap 

`nmap -sC -sV devvortex.htb`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
    |   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
    |_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-title: DevVortex
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


# The Sticker Shop Write-up

<img src="https://tryhackme-images.s3.amazonaws.com/room-icons/618b3fa52f0acc0061fb0172-1718377390091" width="200" height="200">

## Recon 

### NMap

`nmap -sC -sV 10.10.225.208`

    PORT     STATE SERVICE    VERSION
    22/tcp   open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 b2:54:8c:e2:d7:67:ab:8f:90:b3:6f:52:c2:73:37:69 (RSA)
    |   256 14:29:ec:36:95:e5:64:49:39:3f:b4:ec:ca:5f:ee:78 (ECDSA)
    |_  256 19:eb:1f:c9:67:92:01:61:0c:14:fe:71:4b:0d:50:40 (ED25519)
    8080/tcp open  http-proxy Werkzeug/3.0.1 Python/3.8.10
    |_http-title: Cat Sticker Shop
    |_http-server-header: Werkzeug/3.0.1 Python/3.8.10
   

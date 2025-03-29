# Code Write-up

<img src="https://labs.hackthebox.com/storage/avatars/55cc3528cd7ad96f67c4f0c715efe286.png" width="200" height="200">

## Recon 

### Port enumeration with NMap

`nmap -sC -sV 10.10.11.62`

    PORT     STATE    SERVICE   VERSION
    22/tcp   open     ssh       OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 b5:b9:7c:c4:50:32:95:bc:c2:65:17:df:51:a2:7a:bd (RSA)
    |   256 94:b5:25:54:9b:68:af:be:40:e1:1d:a8:6b:85:0d:01 (ECDSA)
    |_  256 12:8c:dc:97:ad:86:00:b4:88:e2:29:cf:69:b5:65:96 (ED25519)
    5000/tcp open     http      Gunicorn 20.0.4
    |_http-title: Python Code Editor
    |_http-server-header: gunicorn/20.0.4
    9898/tcp filtered monkeycom
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

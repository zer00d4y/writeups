# BigBang Write-up

<img src="https://labs.hackthebox.com/storage/avatars/2d22afd496c5ae6f6c51ca24bf3719e1.png" width="200" height="200">

## Recon

### Nmap

`nmap -sC -sV 10.10.11.52`    

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 d4:15:77:1e:82:2b:2f:f1:cc:96:c6:28:c1:86:6b:3f (ECDSA)
    |_  256 6c:42:60:7b:ba:ba:67:24:0f:0c:ac:5d:be:92:0c:66 (ED25519)
    80/tcp open  http    Apache httpd 2.4.62
    |_http-title: Did not follow redirect to http://blog.bigbang.htb/
    |_http-server-header: Apache/2.4.62 (Debian)
    Service Info: Host: blog.bigbang.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add domains to `/etc/hosts`

    echo "10.10.11.52 bigbang.htb" | sudo tee -a /etc/hosts

    echo "10.10.11.52 blog.bigbang.htb" | sudo tee -a /etc/hosts

blog.bigbang.htb

![image](https://github.com/user-attachments/assets/c5455da5-9f58-4757-a2dd-41d373f7932f)


Wappalyzer 

![image](https://github.com/user-attachments/assets/f441cb34-f3ae-465a-8d99-085a87613c17)

## Exploitation 




### WPScan

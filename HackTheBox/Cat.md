# Cat Write-up

<img src="https://labs.hackthebox.com/storage/avatars/bf7ae27f4e0ce1703bdd10d538334d9e.png" width="200" height="200">

## Recon

### Nmap

`nmap -sC -sV 10.10.11.53`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 96:2d:f5:c6:f6:9f:59:60:e5:65:85:ab:49:e4:76:14 (RSA)
    |   256 9e:c4:a4:40:e9:da:cc:62:d1:d6:5a:2f:9e:7b:d4:aa (ECDSA)
    |_  256 6e:22:2a:6a:6d:eb:de:19:b7:16:97:c2:7e:89:29:d5 (ED25519)
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    |_http-title: Did not follow redirect to http://cat.htb/
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
Add domain to `/etc/hosts`

    echo "10.10.11.53 cat.htb" | sudo tee -a /etc/hosts

![image](https://github.com/user-attachments/assets/5dd9fc64-1a8d-40c2-b5a6-49346be6b6c9)

Join page:

![image](https://github.com/user-attachments/assets/0fe84359-e0c5-4f77-a15e-de901e85f170)

![image](https://github.com/user-attachments/assets/9de8d8a4-dffa-4463-8201-5019e3428679)

### Dirsearch 

`python3 dirsearch.py -u http://cat.htb `

    Scanning: 
    301 -   301B - /.git  ->  http://cat.htb/.git/                    
    403 -   272B - /.git/                                             
    403 -   272B - /.git/branches/
    200 -     7B - /.git/COMMIT_EDITMSG
    200 -    73B - /.git/description
    200 -    23B - /.git/HEAD
    200 -    92B - /.git/config                                       
    403 -   272B - /.git/hooks/                                       
    200 -    2KB - /.git/index                                        
    403 -   272B - /.git/info/
    200 -   240B - /.git/info/exclude                                 
    403 -   272B - /.git/logs/                                        
    200 -   150B - /.git/logs/HEAD                                    
    301 -   311B - /.git/logs/refs  ->  http://cat.htb/.git/logs/refs/
    301 -   317B - /.git/logs/refs/heads  ->  http://cat.htb/.git/logs/refs/heads/
    200 -   150B - /.git/logs/refs/heads/master
    403 -   272B - /.git/objects/                                     
    403 -   272B - /.git/refs/                                        
    301 -   312B - /.git/refs/heads  ->  http://cat.htb/.git/refs/heads/
    200 -    41B - /.git/refs/heads/master
    301 -   311B - /.git/refs/tags  ->  http://cat.htb/.git/refs/tags/
    403 -   272B - /.php                                              
    302 -     1B - /admin.php  ->  /join.php                          
    200 -     1B - /config.php                                        
    301 -   300B - /css  ->  http://cat.htb/css/                      
    301 -   300B - /img  ->  http://cat.htb/img/                      
    200 -    3KB - /index.php                                         
    200 -    3KB - /index.php/login/                                  
    302 -     0B - /logout.php  ->  /                                 
    403 -   272B - /server-status                                     
    403 -   272B - /server-status/                                    
    301 -   304B - /uploads  ->  http://cat.htb/uploads/              
    403 -   272B - /uploads/ 

### Stored XSS

![image](https://github.com/user-attachments/assets/5e48bf68-154f-4cb0-ba5a-0668587c1949)

### SQL Injection



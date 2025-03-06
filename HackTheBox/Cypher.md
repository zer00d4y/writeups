# Cypher Write-up

<img src="https://labs.hackthebox.com/storage/avatars/765cd4be6f3a366ca83c7ea60bbcaaa8.png" width="200" height="200">

## Recon 

### Port enumeration with NMap

`nmap -sC -sV 10.10.11.57`

    PORT     STATE    SERVICE VERSION
    22/tcp   open     ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.8 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 be:68:db:82:8e:63:32:45:54:46:b7:08:7b:3b:52:b0 (ECDSA)
    |_  256 e5:5b:34:f5:54:43:93:f8:7e:b6:69:4c:ac:d6:3d:23 (ED25519)
    80/tcp   open     http    nginx 1.24.0 (Ubuntu)
    |_http-title: Did not follow redirect to http://cypher.htb/
    |_http-server-header: nginx/1.24.0 (Ubuntu)
    1501/tcp filtered sas-3
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add ip and domain to `/etc/hosts`

    echo "10.10.11.57 cypher.htb" | sudo tee -a /etc/hosts

### Dirsearch 

`python3 dirsearch.py -u http://cypher.htb --exclude-status 403,404,400,401 -o dir`

    Scanning:
    200 -    5KB - /about                                            
    200 -    5KB - /about.html                                       
    307 -     0B - /api  ->  /api/docs                               
    307 -     0B - /api/  ->  http://cypher.htb/api/api              
    307 -     0B - /demo  ->  /login                                 
    307 -     0B - /demo/  ->  http://cypher.htb/api/demo            
    200 -    4KB - /index                                            
    200 -    4KB - /index.html                                       
    200 -    4KB - /login                                            
    200 -    4KB - /login.html                                       
    301 -   178B - /testing  ->  http://cypher.htb/testing/     

![image](https://github.com/user-attachments/assets/4d473700-502b-4f32-a0fe-a0ea3a14d544)





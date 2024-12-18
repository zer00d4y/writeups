# Sightless Write-up

<img src="https://labs.hackthebox.com/storage/avatars/f96160a20e9cf0138885238444b47404.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.32`

    PORT   STATE SERVICE VERSION
    21/tcp open  ftp
    | fingerprint-strings: 
    |   GenericLines: 
    |     220 ProFTPD Server (sightless.htb FTP Server) [::ffff:10.10.11.32]
    |     Invalid command: try being more creative
    |_    Invalid command: try being more creative
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 c9:6e:3b:8f:c6:03:29:05:e5:a0:ca:00:90:c9:5c:52 (ECDSA)
    |_  256 9b:de:3a:27:77:3b:1b:e1:19:5f:16:11:be:70:e0:56 (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    |_http-title: Did not follow redirect to http://sightless.htb/
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
    SF-Port21-TCP:V=7.94SVN%I=7%D=9/13%Time=66E4778F%P=x86_64-pc-linux-gnu%r(G
    SF:enericLines,A0,"220\x20ProFTPD\x20Server\x20\(sightless\.htb\x20FTP\x20
    SF:Server\)\x20\[::ffff:10\.10\.11\.32\]\r\n500\x20Invalid\x20command:\x20
    SF:try\x20being\x20more\x20creative\r\n500\x20Invalid\x20command:\x20try\x
    SF:20being\x20more\x20creative\r\n");
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Add `sightless.htb` address to `/etc/hosts`

    echo "10.10.11.32 sightless.htb" | sudo tee -a /etc/hosts  

sightless.htb

![image](https://github.com/user-attachments/assets/458c45f0-8807-4df4-a124-72b0bb34433a)

![image](https://github.com/user-attachments/assets/b1dd73b5-b6e7-4b93-8d4c-11ffbeac1b0a)

Add `sqlpad` sub-domain to `/etc/hosts`

    echo "10.10.11.32 sqlpad.sightless.htb" | sudo tee -a /etc/hosts 

![image](https://github.com/user-attachments/assets/51a022a9-4317-45e9-aae4-6a7bd9009c4a)

## Exploitation 

looking for an exploit for SQLpad and found `CVE-2022-0944`

Template injection in connection test endpoint leads to RCE in GitHub repository sqlpad/sqlpad prior to 6.10.1.

### CVE-2022-0944

![image](https://github.com/user-attachments/assets/f9181a40-0516-4487-8ec4-b31e559ffe0d)

![image](https://github.com/user-attachments/assets/a53607cb-511d-49d9-aa37-3729d65a81b6)

![image](https://github.com/user-attachments/assets/c1057d75-adf2-405d-b343-a713ef47c5d7)

![image](https://github.com/user-attachments/assets/fa8f6361-6a99-46b1-a903-437b720231a8)

    hashcat -m 1800 -a 0 hash /home/kali/wordlist/rockyou.txt

Using mode `1800` (sha512crypt $6$, SHA512 (Unix)) for hashcat

`$6$jn8fwk6LVJ9IYw30$qwtrfWTITUro8fEJbReUc7nXyx2wwJsnYdZYm9nMQDHP8SYm33uisO9gZ20LGaepC3ch6Bb2z/lEpBM90Ra4b.`:`blindside`
`$6$mG3Cp2VPGY.FDE8u$KVWVIHzqTzhOSYkzJIpFc2EsgmqvPa.q2Z9bLUU6tlBWaEwuxCDEP9UFHIXNUcF2rBnsaFYuJa6DUh/pL2IJD/`:`insaneclownposse`

Now we get `root` and `michael` passwords, try connecting to SSH with this credentials

## User Flag 

Connect via SSH and get the user flag!

![image](https://github.com/user-attachments/assets/7de140d9-87ba-4002-947a-cec108f2e333)

## Privilege escalation

    netstat -tnlp

![image](https://github.com/user-attachments/assets/c870fb88-b5ff-4009-aaed-6219c003061c)

We can see open 8080 port on internal network 

So, try to portforward

    ssh michael@10.10.11.32  -L 127.0.0.1:8080:127.0.0.1:8080

![image](https://github.com/user-attachments/assets/30f6bc1f-f6bb-413f-b27d-4fb0ddb8db49)

127.0.0.1:8080

![image](https://github.com/user-attachments/assets/20e15a94-b507-49bf-889a-66931c661f60)

    ssh michael@10.10.11.32 \
    -L 127.0.0.1:8080:127.0.0.1:8080 \
    -L 127.0.0.1:41467:127.0.0.1:41467 \
    -L 127.0.0.1:39273:127.0.0.1:39273 \
    -L 127.0.0.1:33060:127.0.0.1:33060 \
    -L 127.0.0.1:3000:127.0.0.1:3000 \
    -L 127.0.0.1:39131:127.0.0.1:39131 \
    -L 127.0.0.1:3306:127.0.0.1:3306 \
    -L 127.0.0.53:53:127.0.0.53:53

![image](https://github.com/user-attachments/assets/2e21b583-4ff2-48c9-81da-129ae46fd51f)

![image](https://github.com/user-attachments/assets/35948cdb-b640-40bf-91bb-bb4d19ead3de)

![image](https://github.com/user-attachments/assets/774d3664-88ae-4b41-b3d1-9d7180c97a69)

PHP -> PHP-FPM versions -> Create new PHP version

![image](https://github.com/user-attachments/assets/4471585c-38d2-4339-9544-64024f5fa32a)

Get the root flag!

![image](https://github.com/user-attachments/assets/41088881-074b-4d72-9ef7-e640d5ade041)


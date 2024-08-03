# BoardLight Write-up

<img src="https://labs.hackthebox.com/storage/avatars/7768afed979c9abe917b0c20df49ceb8.png" width="200" height="200">

## Recon

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.11`

    PORT     STATE    SERVICE          VERSION
    22/tcp   open     ssh              OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 06:2d:3b:85:10:59:ff:73:66:27:7f:0e:ae:03:ea:f4 (RSA)
    |   256 59:03:dc:52:87:3a:35:99:34:44:74:33:78:31:35:fb (ECDSA)
    |_  256 ab:13:38:e4:3e:e0:24:b4:69:38:a9:63:82:38:dd:f4 (ED25519)
    80/tcp   open     http             Apache httpd 2.4.41 ((Ubuntu))
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    |_http-title: Site doesn't have a title (text/html; charset=UTF-8).
    3269/tcp filtered globalcatLDAPssl
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Sub-domain enumeration with ffuf

`crm.board.htb`
    

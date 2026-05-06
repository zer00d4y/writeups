# Team Write-up

## Recon

### Nmap

`nmap -sV -sC 10.113.178.3`

    PORT   STATE SERVICE VERSION
    21/tcp open  ftp     vsftpd 3.0.5
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    |_http-title: Apache2 Ubuntu Default Page: It works! If you see this add 'te...
    MAC Address: 06:7F:B8:59:CC:23 (Unknown)
    Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Add ip to `/etc/hosts`

### Dirsearch

`dirsearch -u http://team.thm/`
    
      _|. _ _  _  _  _ _|_    v0.4.3.post1
     (_||| _) (/_(_|| (_| )
    
    Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25
    Wordlist size: 11460
    
    Target: http://team.thm/
    
    Starting: 
    301 -  305B  - /assets  ->  http://team.thm/assets/
    301 -  305B  - /images  ->  http://team.thm/images/
    200 -  371B  - /images/
    200 -    5B  - /robots.txt
    301 -  306B  - /scripts  ->  http://team.thm/scripts/


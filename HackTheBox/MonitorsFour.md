# MonitorsFour Write-up

<img src="https://htb-mp-prod-public-storage.s3.eu-central-1.amazonaws.com/avatars/c7878dd8dba2eb248a89584ec958a5b8.png" width="200" height="200">

## Recon

### Port enumeration with `Nmap`

`nmap -sC -sV 10.129.2.14`

    PORT     STATE SERVICE VERSION
    80/tcp   open  http    nginx
    |_http-title: Did not follow redirect to http://monitorsfour.htb/
    5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-title: Not Found
    |_http-server-header: Microsoft-HTTPAPI/2.0
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Add host to `/etc/hosts`

    echo "10.129.2.14 monitorsfour.htb" | sudo tee -a /etc/hosts

monitorsfour.htb

<img width="1446" height="730" alt="image" src="https://github.com/user-attachments/assets/1074c829-cde7-46cb-83e1-10b01e367e4b" />

### Directory enumeration with `Dirsearch`

`dirsearch -e php,html,js -u http://monitorsfour.htb 404,403,401,307`

    200 -   97B  - /.env  
    200 -  367B  - /contact 
    200 -    4KB - /login
    200   367B   http://monitorsfour.htb/contact
    200     4KB  http://monitorsfour.htb/login
    301   162B   http://monitorsfour.htb/static    -> REDIRECTS TO: http://monitorsfour.htb/static/
    200    35B   http://monitorsfour.htb/user
    301   162B   http://monitorsfour.htb/views    -> REDIRECTS TO: http://monitorsfour.htb/views/

So now we know about open `.env` file, let's visit page and get it! 

http://monitorsfour.htb/.env

Read `.env` file 

    DB_HOST=mariadb
    DB_PORT=3306
    DB_NAME=monitorsfour_db
    DB_USER=monitorsdbuser
    DB_PASS=f37p2j8f4t0r

### Sub-domain enumeration with `FFUF`

`ffuf -c -u http://monitorsfour.htb/ -H "Host: FUZZ.monitorsfour.htb" -w /usr/share/wordlists/amass/subdomains-top1mil-20000.txt` 

    cacti                   [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 820ms]

Add sub-domain `cacti.monitorsfour.htb` to `/etc/hosts` 

<img width="562" height="431" alt="image" src="https://github.com/user-attachments/assets/9a57f305-90e3-439c-806a-44dce048a0f1" />

## CVE-2024-42179

`5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)`

HCL MyXalytics is affected by sensitive information disclosure vulnerability. 

The HTTP response header exposes the Microsoft-HTTP API∕2.0 as the server's name & version.

## CVE-2024–43363
`Version 1.2.28 | (c) 2004-2026 - The Cacti Group`

Cacti is an open source performance and fault management framework. 

An admin user can create a device with a malicious hostname containing php code and repeat the installation process to use a php file as the cacti log file. 

After having the malicious hostname end up in the logs (log poisoning), one can simply go to the log file url to execute commands to achieve RCE. This issue has been addressed in version 1.2.28 and all users are advised to upgrade. There are no known workarounds for this vulnerability.

## CVE-2025–24367

## Privilege escalation


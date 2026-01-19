# MonitorsFour Write-up

<img src="https://htb-mp-prod-public-storage.s3.eu-central-1.amazonaws.com/avatars/c7878dd8dba2eb248a89584ec958a5b8.png" width="200" height="200">

## Recon

### Port enumeration with Nmap

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

### Directory enumeration with Dirsearch

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



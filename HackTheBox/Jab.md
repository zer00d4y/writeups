# Jab Write-up

<img src="https://labs.hackthebox.com/storage/avatars/e809e8df8d66ec8bb2ca3bbcc1942de7.png" width="200" height="200">

---------------------------------------------------------------------------------------------------------------------

## Nmap

`nmap -sC -sV jab.htb`

    PORT     STATE SERVICE             VERSION
    53/tcp   open  domain              Simple DNS Plus
    88/tcp   open  kerberos-sec        Microsoft Windows Kerberos (server time: 2024-03-04 08:42:51Z)
    135/tcp  open  msrpc               Microsoft Windows RPC
    139/tcp  open  netbios-ssn         Microsoft Windows netbios-ssn
    389/tcp  open  ldap                Microsoft Windows Active Directory LDAP (Domain: jab.htb0., Site: Default-First-Site-Name)
    | ssl-cert: Subject: commonName=DC01.jab.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.jab.htb
    | Not valid before: 2023-11-01T20:16:18
    |_Not valid after:  2024-10-31T20:16:18
    |_ssl-date: 2024-03-04T08:43:45+00:00; -2s from scanner time.
    445/tcp  open  microsoft-ds?
    464/tcp  open  kpasswd5?
    593/tcp  open  ncacn_http          Microsoft Windows RPC over HTTP 1.0
    636/tcp  open  ssl/ldap            Microsoft Windows Active Directory LDAP (Domain: jab.htb0., Site: Default-First-Site-Name)
    |_ssl-date: 2024-03-04T08:43:45+00:00; -2s from scanner time.
    | ssl-cert: Subject: commonName=DC01.jab.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.jab.htb
    | Not valid before: 2023-11-01T20:16:18
    |_Not valid after:  2024-10-31T20:16:18
    3268/tcp open  ldap                Microsoft Windows Active Directory LDAP (Domain: jab.htb0., Site: Default-First-Site-Name)
    |_ssl-date: 2024-03-04T08:43:47+00:00; -2s from scanner time.
    | ssl-cert: Subject: commonName=DC01.jab.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.jab.htb
    | Not valid before: 2023-11-01T20:16:18
    |_Not valid after:  2024-10-31T20:16:18
    3269/tcp open  ssl/ldap            Microsoft Windows Active Directory LDAP (Domain: jab.htb0., Site: Default-First-Site-Name)
    |_ssl-date: 2024-03-04T08:43:45+00:00; -2s from scanner time.
    | ssl-cert: Subject: commonName=DC01.jab.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.jab.htb
    | Not valid before: 2023-11-01T20:16:18
    |_Not valid after:  2024-10-31T20:16:18
    5222/tcp open  jabber
    |_ssl-date: TLS randomness does not represent time
    | xmpp-info: 
    |   STARTTLS Failed
    |   info: 
    |     stream_id: 1rxvz70nyd
    |     unknown: 
    |     capabilities: 
    |     errors: 
    |       invalid-namespace
    |       (timeout)
    |     auth_mechanisms: 
    |     features: 
    |     compression_methods: 
    |     xmpp: 
    |_      version: 1.0
    | fingerprint-strings: 
    |   RPCCheck: 
    |_    <stream:error xmlns:stream="http://etherx.jabber.org/streams"><not-well-formed xmlns="urn:ietf:params:xml:ns:xmpp-streams"/></stream:error></stream:stream>
    | ssl-cert: Subject: commonName=dc01.jab.htb
    | Subject Alternative Name: DNS:dc01.jab.htb, DNS:*.dc01.jab.htb
    | Not valid before: 2023-10-26T22:00:12
    |_Not valid after:  2028-10-24T22:00:12
    5269/tcp open  xmpp                Wildfire XMPP Client
    | xmpp-info: 
    |   STARTTLS Failed
    |   info: 
    |     xmpp: 
    |     capabilities: 
    |     errors: 
    |       (timeout)
    |     auth_mechanisms: 
    |     features: 
    |     unknown: 
    |_    compression_methods: 
    7070/tcp open  realserver?
    | fingerprint-strings: 
    |   DNSStatusRequestTCP, DNSVersionBindReqTCP: 
    |     HTTP/1.1 400 Illegal character CNTL=0x0
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 69
    |     Connection: close
    |     <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x0</pre>
    |   GetRequest: 
    |     HTTP/1.1 200 OK
    |     Date: Mon, 04 Mar 2024 08:42:51 GMT
    |     Last-Modified: Wed, 16 Feb 2022 15:55:02 GMT
    |     Content-Type: text/html
    |     Accept-Ranges: bytes
    |     Content-Length: 223
    |     <html>
    |     <head><title>Openfire HTTP Binding Service</title></head>
    |     <body><font face="Arial, Helvetica"><b>Openfire <a href="http://www.xmpp.org/extensions/xep-0124.html">HTTP Binding</a> Service</b></font></body>
    |     </html>
    |   HTTPOptions: 
    |     HTTP/1.1 200 OK
    |     Date: Mon, 04 Mar 2024 08:42:57 GMT
    |     Allow: GET,HEAD,POST,OPTIONS
    |   Help: 
    |     HTTP/1.1 400 No URI
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 49
    |     Connection: close
    |     <h1>Bad Message 400</h1><pre>reason: No URI</pre>
    |   RPCCheck: 
    |     HTTP/1.1 400 Illegal character OTEXT=0x80
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 71
    |     Connection: close
    |     <h1>Bad Message 400</h1><pre>reason: Illegal character OTEXT=0x80</pre>
    |   RTSPRequest: 
    |     HTTP/1.1 505 Unknown Version
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 58
    |     Connection: close
    |     <h1>Bad Message 505</h1><pre>reason: Unknown Version</pre>
    |   SSLSessionReq: 
    |     HTTP/1.1 400 Illegal character CNTL=0x16
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 70
    |     Connection: close
    |_    <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x16</pre>
    7443/tcp open  ssl/oracleas-https?
    | fingerprint-strings: 
    |   DNSStatusRequestTCP, DNSVersionBindReqTCP: 
    |     HTTP/1.1 400 Illegal character CNTL=0x0
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 69
    |     Connection: close
    |     <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x0</pre>
    |   GetRequest: 
    |     HTTP/1.1 200 OK
    |     Date: Mon, 04 Mar 2024 08:42:58 GMT
    |     Last-Modified: Wed, 16 Feb 2022 15:55:02 GMT
    |     Content-Type: text/html
    |     Accept-Ranges: bytes
    |     Content-Length: 223
    |     <html>
    |     <head><title>Openfire HTTP Binding Service</title></head>
    |     <body><font face="Arial, Helvetica"><b>Openfire <a href="http://www.xmpp.org/extensions/xep-0124.html">HTTP Binding</a> Service</b></font></body>
    |     </html>
    |   HTTPOptions: 
    |     HTTP/1.1 200 OK
    |     Date: Mon, 04 Mar 2024 08:43:04 GMT
    |     Allow: GET,HEAD,POST,OPTIONS
    |   Help: 
    |     HTTP/1.1 400 No URI
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 49
    |     Connection: close
    |     <h1>Bad Message 400</h1><pre>reason: No URI</pre>
    |   RPCCheck: 
    |     HTTP/1.1 400 Illegal character OTEXT=0x80
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 71
    |     Connection: close
    |     <h1>Bad Message 400</h1><pre>reason: Illegal character OTEXT=0x80</pre>
    |   RTSPRequest: 
    |     HTTP/1.1 505 Unknown Version
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 58
    |     Connection: close
    |     <h1>Bad Message 505</h1><pre>reason: Unknown Version</pre>
    |   SSLSessionReq: 
    |     HTTP/1.1 400 Illegal character CNTL=0x16
    |     Content-Type: text/html;charset=iso-8859-1
    |     Content-Length: 70
    |     Connection: close
    |_    <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x16</pre>
    |_ssl-date: TLS randomness does not represent time
    | ssl-cert: Subject: commonName=dc01.jab.htb
    | Subject Alternative Name: DNS:dc01.jab.htb, DNS:*.dc01.jab.htb
    | Not valid before: 2023-10-26T22:00:12
    |_Not valid after:  2028-10-24T22:00:12
    7777/tcp open  socks5              (No authentication; connection not allowed by ruleset)
    | socks-auth-info: 
    |_  No authentication
    3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
    ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
    SF-Port5222-TCP:V=7.94SVN%I=7%D=3/4%Time=65E589A2%P=x86_64-pc-linux-gnu%r(
    SF:RPCCheck,9B,"<stream:error\x20xmlns:stream=\"http://etherx\.jabber\.org
    SF:/streams\"><not-well-formed\x20xmlns=\"urn:ietf:params:xml:ns:xmpp-stre
    SF:ams\"/></stream:error></stream:stream>");
    ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
    SF-Port7070-TCP:V=7.94SVN%I=7%D=3/4%Time=65E5898D%P=x86_64-pc-linux-gnu%r(
    SF:GetRequest,189,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Mon,\x2004\x20Mar\x2
    SF:02024\x2008:42:51\x20GMT\r\nLast-Modified:\x20Wed,\x2016\x20Feb\x202022
    SF:\x2015:55:02\x20GMT\r\nContent-Type:\x20text/html\r\nAccept-Ranges:\x20
    SF:bytes\r\nContent-Length:\x20223\r\n\r\n<html>\n\x20\x20<head><title>Ope
    SF:nfire\x20HTTP\x20Binding\x20Service</title></head>\n\x20\x20<body><font
    SF:\x20face=\"Arial,\x20Helvetica\"><b>Openfire\x20<a\x20href=\"http://www
    SF:\.xmpp\.org/extensions/xep-0124\.html\">HTTP\x20Binding</a>\x20Service<
    SF:/b></font></body>\n</html>\n")%r(RTSPRequest,AD,"HTTP/1\.1\x20505\x20Un
    SF:known\x20Version\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\nCo
    SF:ntent-Length:\x2058\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message\x
    SF:20505</h1><pre>reason:\x20Unknown\x20Version</pre>")%r(HTTPOptions,56,"
    SF:HTTP/1\.1\x20200\x20OK\r\nDate:\x20Mon,\x2004\x20Mar\x202024\x2008:42:5
    SF:7\x20GMT\r\nAllow:\x20GET,HEAD,POST,OPTIONS\r\n\r\n")%r(RPCCheck,C7,"HT
    SF:TP/1\.1\x20400\x20Illegal\x20character\x20OTEXT=0x80\r\nContent-Type:\x
    SF:20text/html;charset=iso-8859-1\r\nContent-Length:\x2071\r\nConnection:\
    SF:x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x
    SF:20character\x20OTEXT=0x80</pre>")%r(DNSVersionBindReqTCP,C3,"HTTP/1\.1\
    SF:x20400\x20Illegal\x20character\x20CNTL=0x0\r\nContent-Type:\x20text/htm
    SF:l;charset=iso-8859-1\r\nContent-Length:\x2069\r\nConnection:\x20close\r
    SF:\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x20characte
    SF:r\x20CNTL=0x0</pre>")%r(DNSStatusRequestTCP,C3,"HTTP/1\.1\x20400\x20Ill
    SF:egal\x20character\x20CNTL=0x0\r\nContent-Type:\x20text/html;charset=iso
    SF:-8859-1\r\nContent-Length:\x2069\r\nConnection:\x20close\r\n\r\n<h1>Bad
    SF:\x20Message\x20400</h1><pre>reason:\x20Illegal\x20character\x20CNTL=0x0
    SF:</pre>")%r(Help,9B,"HTTP/1\.1\x20400\x20No\x20URI\r\nContent-Type:\x20t
    SF:ext/html;charset=iso-8859-1\r\nContent-Length:\x2049\r\nConnection:\x20
    SF:close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20No\x20URI</p
    SF:re>")%r(SSLSessionReq,C5,"HTTP/1\.1\x20400\x20Illegal\x20character\x20C
    SF:NTL=0x16\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\nContent-Le
    SF:ngth:\x2070\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h
    SF:1><pre>reason:\x20Illegal\x20character\x20CNTL=0x16</pre>");
    ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
    SF-Port7443-TCP:V=7.94SVN%T=SSL%I=7%D=3/4%Time=65E58994%P=x86_64-pc-linux-
    SF:gnu%r(GetRequest,189,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Mon,\x2004\x20
    SF:Mar\x202024\x2008:42:58\x20GMT\r\nLast-Modified:\x20Wed,\x2016\x20Feb\x
    SF:202022\x2015:55:02\x20GMT\r\nContent-Type:\x20text/html\r\nAccept-Range
    SF:s:\x20bytes\r\nContent-Length:\x20223\r\n\r\n<html>\n\x20\x20<head><tit
    SF:le>Openfire\x20HTTP\x20Binding\x20Service</title></head>\n\x20\x20<body
    SF:><font\x20face=\"Arial,\x20Helvetica\"><b>Openfire\x20<a\x20href=\"http
    SF:://www\.xmpp\.org/extensions/xep-0124\.html\">HTTP\x20Binding</a>\x20Se
    SF:rvice</b></font></body>\n</html>\n")%r(HTTPOptions,56,"HTTP/1\.1\x20200
    SF:\x20OK\r\nDate:\x20Mon,\x2004\x20Mar\x202024\x2008:43:04\x20GMT\r\nAllo
    SF:w:\x20GET,HEAD,POST,OPTIONS\r\n\r\n")%r(RTSPRequest,AD,"HTTP/1\.1\x2050
    SF:5\x20Unknown\x20Version\r\nContent-Type:\x20text/html;charset=iso-8859-
    SF:1\r\nContent-Length:\x2058\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Me
    SF:ssage\x20505</h1><pre>reason:\x20Unknown\x20Version</pre>")%r(RPCCheck,
    SF:C7,"HTTP/1\.1\x20400\x20Illegal\x20character\x20OTEXT=0x80\r\nContent-T
    SF:ype:\x20text/html;charset=iso-8859-1\r\nContent-Length:\x2071\r\nConnec
    SF:tion:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Ill
    SF:egal\x20character\x20OTEXT=0x80</pre>")%r(DNSVersionBindReqTCP,C3,"HTTP
    SF:/1\.1\x20400\x20Illegal\x20character\x20CNTL=0x0\r\nContent-Type:\x20te
    SF:xt/html;charset=iso-8859-1\r\nContent-Length:\x2069\r\nConnection:\x20c
    SF:lose\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x20ch
    SF:aracter\x20CNTL=0x0</pre>")%r(DNSStatusRequestTCP,C3,"HTTP/1\.1\x20400\
    SF:x20Illegal\x20character\x20CNTL=0x0\r\nContent-Type:\x20text/html;chars
    SF:et=iso-8859-1\r\nContent-Length:\x2069\r\nConnection:\x20close\r\n\r\n<
    SF:h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x20character\x20CN
    SF:TL=0x0</pre>")%r(Help,9B,"HTTP/1\.1\x20400\x20No\x20URI\r\nContent-Type
    SF::\x20text/html;charset=iso-8859-1\r\nContent-Length:\x2049\r\nConnectio
    SF:n:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20No\x20
    SF:URI</pre>")%r(SSLSessionReq,C5,"HTTP/1\.1\x20400\x20Illegal\x20characte
    SF:r\x20CNTL=0x16\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\nCont
    SF:ent-Length:\x2070\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message\x20
    SF:400</h1><pre>reason:\x20Illegal\x20character\x20CNTL=0x16</pre>");
    Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-time: 
    |   date: 2024-03-04T08:43:36
    |_  start_date: N/A
    |_clock-skew: mean: -2s, deviation: 0s, median: -2s
    | smb2-security-mode: 
    |   3:1:1: 
    |_    Message signing enabled and required


# Fluffy Write-up

`nmap -sC -sV 10.10.11.69`

    PORT     STATE SERVICE           VERSION
    53/tcp   open  domain            Simple DNS Plus
    88/tcp   open  kerberos-sec      Microsoft Windows Kerberos (server time: 2025-05-28 00:34:31Z)
    139/tcp  open  netbios-ssn       Microsoft Windows netbios-ssn
    389/tcp  open  ldap
    |_ssl-date: TLS randomness does not represent time
    | ssl-cert: Subject: commonName=DC01.fluffy.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.fluffy.htb
    | Not valid before: 2025-04-17T16:04:17
    |_Not valid after:  2026-04-17T16:04:17                                                                                                               
    445/tcp  open  microsoft-ds?
    464/tcp  open  kpasswd5? 
    593/tcp  open  ncacn_http        Microsoft Windows RPC over HTTP 1.0                                                                                   
    636/tcp  open  ldapssl? 
    | ssl-cert: Subject: commonName=DC01.fluffy.htb   
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.fluffy.htb  
    | Not valid before: 2025-04-17T16:04:17  
    |_Not valid after:  2026-04-17T16:04:17         
    3268/tcp open  ldap  
    | ssl-cert: Subject: commonName=DC01.fluffy.htb       
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.fluffy.htb  
    | Not valid before: 2025-04-17T16:04:17          
    |_Not valid after:  2026-04-17T16:04:17     
    |_ssl-date: 2025-05-28T00:36:33+00:00; +7h02m42s from scanner time.    
    3269/tcp open  globalcatLDAPssl?    
    | ssl-cert: Subject: commonName=DC01.fluffy.htb    
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.fluffy.htb   
    | Not valid before: 2025-04-17T16:04:17 
    |_Not valid after:  2026-04-17T16:04:17   
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows    
    
    Host script results:                                                                           
    |_smb2-time: Protocol negotiation failed (SMB2)                          
    |_clock-skew: 7h02m41s

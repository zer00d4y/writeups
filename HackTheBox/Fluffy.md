# Fluffy Write-up

<img src="https://labs.hackthebox.com/storage/avatars/ef8fc92ac7cccd8afa4412241432f064.png" width="200" height="200"> 

MACHINE INFORMATION

As is common in real life Windows pentests, you will start the Fluffy box with credentials for the following account: 

`j.fleischman` / `J0elTHEM4n1990!`

## Recon

### Port enumeration with NMap

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

Add to `/etc/hosts`

    echo "10.10.11.69 DC01.fluffy.htb fluffy.htb" | sudo tee -a /etc/hosts

### SMB enumeration

`smbmap -H 10.10.11.69 -u 'j.fleischman' -p 'J0elTHEM4n1990!'` 
    
        ________  ___      ___  _______   ___      ___       __         _______
       /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
      (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
       \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
        __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
       /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
      (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
    -----------------------------------------------------------------------------
    SMBMap - Samba Share Enumerator v1.10.4 | Shawn Evans - ShawnDEvans@gmail.com<mailto:ShawnDEvans@gmail.com>
                         https://github.com/ShawnDEvans/smbmap
    
    [*] Detected 1 hosts serving SMB                                                                                                  
    [*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                          
                                                                                                                                 
    [+] IP: 10.10.11.69:445 Name: DC01.fluffy.htb           Status: Authenticated
            Disk                                                    Permissions     Comment
            ----                                                    -----------     -------
            ADMIN$                                                  NO ACCESS       Remote Admin
            C$                                                      NO ACCESS       Default share
            IPC$                                                    READ ONLY       Remote IPC
            IT                                                      READ, WRITE
            NETLOGON                                                READ ONLY       Logon server share 
            SYSVOL                                                  READ ONLY       Logon server share 
    [*] Closed 1 connections                                                                          




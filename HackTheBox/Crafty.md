# Crafty Write-up

<img src="https://labs.hackthebox.com/storage/avatars/c01c8813bfc7795ae0717bbee7b407d1.png" width="200" height="200">

## Nmap enumeration

`nmap -sC -sV -p- crafty.htb`

    ┌──(root㉿kali)-[/home/kali]
    └─# nmap -sC -sV -p- crafty.htb      
    
    PORT      STATE SERVICE   VERSION
    80/tcp    open  http      Microsoft IIS httpd 10.0
    |_http-server-header: Microsoft-IIS/10.0
    |_http-title: Crafty - Official Website
    | http-methods: 
    |_  Potentially risky methods: TRACE
    25565/tcp open  minecraft Minecraft 1.16.5 (Protocol: 127, Message: Crafty Server, Users: 1/100)
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

![image](https://github.com/zer00d4y/writeups/assets/128820441/bc60baa0-a1e2-40ef-b3c7-5fbd03a921e9)



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

## log4j

CVE-2021-44228

Exploit link: https://github.com/kozmer/log4j-shell-poc

`git clone https://github.com/kozmer/log4j-shell-poc`

![image](https://github.com/zer00d4y/writeups/assets/128820441/80440668-29ee-40e6-ae42-8abbdd022779)

pyCraft: https://github.com/ammaraskar/pyCraft

`git clone https://github.com/ammaraskar/pyCraft`

![image](https://github.com/zer00d4y/writeups/assets/128820441/63123523-fa92-4b18-8afa-ba62338c33d9)

![image](https://github.com/zer00d4y/writeups/assets/128820441/ac4e62ec-0683-441b-a931-0bfa086e50d9)


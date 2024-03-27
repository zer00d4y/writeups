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

    git clone https://github.com/kozmer/log4j-shell-poc

![image](https://github.com/zer00d4y/writeups/assets/128820441/eaa658db-c1b1-4d6c-bc3f-130bf8757f81)

    wget https://repo.huaweicloud.com/java/jdk/8u181-b13/jdk-8u181-linux-x64.tar.gz

![image](https://github.com/zer00d4y/writeups/assets/128820441/8ef7aeb1-7ae7-407a-a7cf-73a53fc36fab)

    tar -xf jdk-8u181-linux-x64.tar.gz
    
    mv jdk1.8.0_181 jdk1.8.0_20

![image](https://github.com/zer00d4y/writeups/assets/128820441/c1a55a94-0106-49d4-8b3c-948dcc0d9564)

![image](https://github.com/zer00d4y/writeups/assets/128820441/80440668-29ee-40e6-ae42-8abbdd022779)

pyCraft: https://github.com/ammaraskar/pyCraft

    git clone https://github.com/ammaraskar/pyCraft

![image](https://github.com/zer00d4y/writeups/assets/128820441/d82509f2-a181-4233-a749-21917ee2ef95)

    virtualenv ENV
    
    source ENV/bin/activate
    
    pip install -r requirements.txt

![image](https://github.com/zer00d4y/writeups/assets/128820441/3a437c19-6323-4f4c-8c1c-6654cc8c7f7b)

![image](https://github.com/zer00d4y/writeups/assets/128820441/e4e4b870-0723-4171-b244-e53c9fc97e89)

![image](https://github.com/zer00d4y/writeups/assets/128820441/b3bb40ac-5e3d-483c-b001-77886fdd8af7)



![image](https://github.com/zer00d4y/writeups/assets/128820441/63123523-fa92-4b18-8afa-ba62338c33d9)

![image](https://github.com/zer00d4y/writeups/assets/128820441/ac4e62ec-0683-441b-a931-0bfa086e50d9)


# SolarLab Write-up

<img src="https://labs.hackthebox.com/storage/avatars/a2c2bd7b4e98ff8b782ed590896305a1.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.16`

    PORT    STATE SERVICE       VERSION
    80/tcp  open  http          nginx 1.24.0
    |_http-server-header: nginx/1.24.0
    |_http-title: Did not follow redirect to http://solarlab.htb/
    135/tcp open  msrpc         Microsoft Windows RPC
    139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
    445/tcp open  microsoft-ds?
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-security-mode: 
    |   3:1:1: 
    |_    Message signing enabled but not required
    | smb2-time: 
    |   date: 2024-05-16T07:39:43
    |_  start_date: N/A

`nmap -sC -sV -p- 10.10.11.16`

    PORT     STATE SERVICE       VERSION
    80/tcp   open  http          nginx 1.24.0
    |_http-title: Did not follow redirect to http://solarlab.htb/
    135/tcp  open  msrpc         Microsoft Windows RPC
    139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
    445/tcp  open  microsoft-ds?
    6791/tcp open  http          nginx 1.24.0
    |_http-title: Did not follow redirect to http://report.solarlab.htb:6791/
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-time: 
    |   date: 2024-05-16T07:46:56
    |_  start_date: N/A
    | smb2-security-mode: 
    |   3:1:1: 
    |_    Message signing enabled but not required

Add `solarlab.htb` and `report.solarlab.htb` domains to `/etc/hosts`

    echo "10.10.11.16 solarlab.htb" | sudo tee -a /etc/hosts

    echo "10.10.11.16 report.solarlab.htb" | sudo tee -a /etc/hosts

solarlab.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/642ba8f4-6346-4b5c-ad1e-09e1bcac52d3)

report.solarlab.htb:6791

![image](https://github.com/zer00d4y/writeups/assets/128820441/68d45567-5ddd-4117-bcea-8840f61d4051)

### SMB

smbclient  //10.10.11.16/Documents -U anonymous

![image](https://github.com/zer00d4y/writeups/assets/128820441/73f52f42-4035-4091-8dbc-d750b3b036f2)

details-file.xlsx

![image](https://github.com/zer00d4y/writeups/assets/128820441/b3e48d13-4d07-4956-a086-5aa183493eef)

    crackmapexec smb solarlab.htb -u anonymous -p '' --rid-brute

![image](https://github.com/zer00d4y/writeups/assets/128820441/2e1e42d1-c198-4a03-82c5-5161a79c2b18)

    crackmapexec smb solarlab.htb -u blake -p pass.txt

![image](https://github.com/zer00d4y/writeups/assets/128820441/e1dbe3ab-afd7-48c1-bc1e-3956a663df90)

Credentials

`blakeb`:`ThisCanB3typedeasily1@`

![image](https://github.com/zer00d4y/writeups/assets/128820441/e9ed66d5-affc-402c-aba9-c26516e7ec11)



# Bizness Write-up

<img src="https://labs.hackthebox.com/storage/avatars/1919b64800f6676d0c0d285a9d664cee.png" width="200" height="200">

## Port enumeration

`nmap -sC -sV -p- bizness.htb`

# CVE 

CVE-2023-49070

About vulnerability: https://nvd.nist.gov/vuln/detail/CVE-2023-49070

Exploit link: https://github.com/abdoghazy2015/ofbiz-CVE-2023-49070-RCE-POC

`git clone https://github.com/abdoghazy2015/ofbiz-CVE-2023-49070-RCE-POC`

You must download ysoserial-all.jar from here

`wget https://github.com/frohoff/ysoserial/releases/latest/download/ysoserial-all.jar`

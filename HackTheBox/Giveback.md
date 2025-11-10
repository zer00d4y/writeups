# Giveback Write-up

## Recon 

### Port enumeration with NMap

`nmap -sC -sV 10.10.11.94`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 66:f8:9c:58:f4:b8:59:bd:cd:ec:92:24:c3:97:8e:9e (ECDSA)
    |_  256 96:31:8a:82:1a:65:9f:0a:a2:6c:ff:4d:44:7c:d3:94 (ED25519)
    80/tcp open  http    nginx 1.28.0
    |_http-server-header: nginx/1.28.0
    |_http-generator: WordPress 6.8.1
    |_http-title: GIVING BACK IS WHAT MATTERS MOST &#8211; OBVI
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

# Facts Write-up

<img src="https://htb-mp-prod-public-storage.s3.eu-central-1.amazonaws.com/avatars/bdcd209c32f156fbfb2268f099971f75.png" width="200" height="200"> 

## Recon

### Port enumeration with Nmap

`nmap -sC -sV facts.htb`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 9.9p1 Ubuntu 3ubuntu3.2 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 4d:d7:b2:8c:d4:df:57:9c:a4:2f:df:c6:e3:01:29:89 (ECDSA)
    |_  256 a3:ad:6b:2f:4a:bf:6f:48:ac:81:b9:45:3f:de:fb:87 (ED25519)
    80/tcp open  http    nginx 1.26.3 (Ubuntu)
    |_http-title: Did not follow redirect to http://facts.htb/
    |_http-server-header: nginx/1.26.3 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

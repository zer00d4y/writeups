# Backfire Write-up

<img src="https://labs.hackthebox.com/storage/avatars/aa0a93908243c51fe21e691fc6571911.png" width="200" height="200">

## Recon

### Port enumeration with Nmap

`nmap -sC -sV 10.10.11.49`

    PORT     STATE SERVICE    VERSION
    22/tcp   open  ssh        OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
    | ssh-hostkey:                                                                                                                                                                                                                              
    |_  256 be:f3:27:9e:c6:d6:29:27:7b:98:18:91:4e:97:25:99 (ED25519)                                                                                                                                                                           
    443/tcp  open  ssl/https?                                                                                                                                                                                                                   
    | ssl-cert: Subject: commonName=127.0.0.1/stateOrProvinceName=Florida/countryName=US                                                                                                                                                        
    | Subject Alternative Name: IP Address:127.0.0.1                                                                                                                                                                                            
    | Not valid before: 2024-05-30T05:03:56                                                                                                                                                                                                     
    |_Not valid after:  2027-05-30T05:03:56                                                                                                                                                                                                     
    |_ssl-date: TLS randomness does not represent time                                                                                                                                                                                          
    | tls-alpn:                                                                                                                                                                                                                                 
    |   http/1.1                                                                                                                                                                                                                                
    |   http/1.0                                                                                                                                                                                                                                
    |_  http/0.9                                                                                                                                                                                                                                
    8000/tcp open  http       nginx 1.22.1                                                                                                                                                                                                      
    |_http-server-header: nginx/1.22.1                                                                                                                                                                                                          
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel  

Use the `-p-` flag and perform a full scan of our target.

`nmap -sC -sV -p- 10.10.11.49`  



havoc.yaotl

![image](https://github.com/user-attachments/assets/9f0e32e3-4fa7-4a70-8865-b90ea8066ae8)

`ilya`:`CobaltStr1keSuckz!`

`sergej`:`1w4nt2sw1tch2h4rdh4tc2`


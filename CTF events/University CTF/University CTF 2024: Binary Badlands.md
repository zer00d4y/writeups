# University CTF 2024: Binary Badlands Write-up

![e4ojfv2PZEZDSPUTlsDZekbXk06ICAXjrcSji4Sa](https://github.com/user-attachments/assets/0fb6bcde-8753-46dc-8400-8258dae97f40)

# Warm-up

## Task - Welcome to CTF

Spawn docker with task and get the flag!

![image](https://github.com/user-attachments/assets/b1acbece-242e-4609-82ec-648e3550d2a3)

## FullPWN

### Task - Apolo

#### NMap

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 48add5b83a9fbcbef7e8201ef6bfdeae (RSA)
    |   256 b7896c0b20ed49b2c1867c2992741c1f (ECDSA)
    |_  256 18cd9d08a621a8b8b6f79f8d405154fb (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    |_http-title: Did not follow redirect to http://apolo.htb/
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

add to `/etc/hosts` 
      
    echo "10.129.231.24 apolo.htb" | sudo tee -a /etc/hosts

landing page

![image](https://github.com/user-attachments/assets/624da67e-5532-4507-8c05-9442bec7f505)

Hyperlink in AI Technology redirects us to `ai.apolo.htb` add to `/etc/hosts`

    sudo sed -i '/10\.129\.231\.24/ s/$/ ai.apolo.htb/' /etc/hosts

This command works by using `sed (stream editor)` to locate the line in `/etc/hosts` containing the IP `10.129.231.24` and appending `ai.apolo.htb` to the end of that line.


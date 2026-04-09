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

## Level 0 → Level 1

    Username: natas1
    URL:      http://natas1.natas.labs.overthewire.org

<img width="499" height="299" alt="image" src="https://github.com/user-attachments/assets/a520ca06-d66c-4c0b-84df-1453c641adf1" />

<img width="1453" height="507" alt="image" src="https://github.com/user-attachments/assets/13e8094e-233d-4b11-b320-6c45e00f8959" />

Use `CTRL + U` to view source page! 

<img width="1132" height="366" alt="image" src="https://github.com/user-attachments/assets/7a4fc59e-c311-4ed9-9ca0-80e0552e3408" />

The password for natas2 is:

    TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI 

## Level 1 → Level 2

    Username: natas2
    URL:      http://natas2.natas.labs.overthewire.org

<img width="1125" height="355" alt="image" src="https://github.com/user-attachments/assets/fb53fbb2-72ca-42a4-81b1-3e67659ac1cf" />

We can see `/files/` directories 

<img width="1116" height="409" alt="image" src="https://github.com/user-attachments/assets/53fa8479-53b4-4f28-b8c9-0683b8d6c112" />

http://natas2.natas.labs.overthewire.org/files/users.txt

<img width="701" height="216" alt="image" src="https://github.com/user-attachments/assets/85047c88-8df1-4092-944d-a036cb2f850b" />

natas3:

    3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH

## Level 2 → Level 3

    Username: natas3
    URL:      http://natas3.natas.labs.overthewire.org

`/robots.txt` page

<img width="703" height="151" alt="image" src="https://github.com/user-attachments/assets/2fee8124-d79e-4909-950c-ba78cc9a6d23" />

`/s3cr3t/` page



`/s3cr3t/user.txt` file

<img width="698" height="145" alt="image" src="https://github.com/user-attachments/assets/057d5b4f-2723-4b39-8f06-fc935947ad97" />

natas4:

    QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

## Level 3 → Level 4

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

## Level 4 → Level 5

    Username: natas5
    URL:      http://natas5.natas.labs.overthewire.org

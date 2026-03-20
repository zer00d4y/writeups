# Bandit Write-up

https://overthewire.org/wargames/bandit

## Level 0

The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. 
The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

    ssh bandit0@bandit.labs.overthewire.org -p 2220

<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/3f229bf3-f659-4867-8801-1b4646733f92" />

## Level 0 → Level 1

The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. 
Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

<img width="719" height="214" alt="image" src="https://github.com/user-attachments/assets/f3e477a0-d5f4-45c1-b3b2-176d86bd7c25" />

Password for next level:

    ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

## Level 1 → Level 2

The password for the next level is stored in a file called `-` located in the home directory

`bandit1`:`ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

    ssh bandit1@bandit.labs.overthewire.org -p 2220

<img width="560" height="188" alt="image" src="https://github.com/user-attachments/assets/e6a62955-2205-46ce-acb1-3ad73b545707" />

Password for next level:

    263JGJPfgU6LtdEvgfWU1XP5yac29mFx

## Level 2 → Level 3

The password for the next level is stored in a file called --spaces in this filename-- located in the home directory

`bandit2`:`263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

    ssh bandit2@bandit.labs.overthewire.org -p 2220

<img width="678" height="191" alt="image" src="https://github.com/user-attachments/assets/510eda03-6a61-4065-b337-abfb0a3a8405" />

Password for next level:

    MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

## Level 3 → Level 4

The password for the next level is stored in a hidden file in the inhere directory.

`bandit3`:`MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

    ssh bandit3@bandit.labs.overthewire.org -p 2220

<img width="581" height="299" alt="image" src="https://github.com/user-attachments/assets/1378a800-24e0-4065-b7c5-14e6fdbfdcd4" />

Password for next level:

    2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

## Level 4 → Level 5

The password for the next level is stored in the only human-readable file in the inhere directory. 
Tip: if your terminal is messed up, try the “reset” command.

`bandit4`:`2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

    ssh bandit4@bandit.labs.overthewire.org -p 2220

<img width="412" height="244" alt="image" src="https://github.com/user-attachments/assets/b332ca91-af09-4e95-9b6d-3b709b2ee72f" />

Password for next level:

    4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

## Level 5 → Level 6

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

human-readable,
1033 bytes in size,
not executable.

`bandit5`:`4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

    ssh bandit5@bandit.labs.overthewire.org -p 2220

<img width="567" height="482" alt="image" src="https://github.com/user-attachments/assets/65690de1-4b69-49bb-bcc1-3b0ef1db1dd4" />

Password for next level:

    HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

## Level 6 → Level 7

The password for the next level is stored somewhere on the server and has all of the following properties:

owned by user bandit7,
owned by group bandit6,
33 bytes in size.

`bandit6`:`HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

    ssh bandit6@bandit.labs.overthewire.org -p 2220

<img width="634" height="98" alt="image" src="https://github.com/user-attachments/assets/c1e053e1-69e5-4eaa-bf3d-c667709ef59c" />

Password for next level:

    morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

## Level 7 → Level 8

The password for the next level is stored in the file data.txt next to the word millionth

`bandit7`:`morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

    ssh bandit7@bandit.labs.overthewire.org -p 2220

<img width="430" height="69" alt="image" src="https://github.com/user-attachments/assets/cd5930ea-3ead-4b0e-9318-104bfdc133cf" />

Password for next level:

    dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

## Level 8 → Level 9

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

`bandit8`:`dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

    ssh bandit8@bandit.labs.overthewire.org -p 2220

<img width="414" height="62" alt="image" src="https://github.com/user-attachments/assets/3961b70e-ad36-477d-82dd-ab3ed01acf74" />

Password for next level:

    4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

## Level 9 → Level 10

The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

`bandit9`:`4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

    ssh bandit9@bandit.labs.overthewire.org -p 2220

<img width="478" height="111" alt="image" src="https://github.com/user-attachments/assets/11d8a035-c7c1-4432-bce2-be42af0bb645" />

Password for next level:

    FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

## Level 10 → Level 11

The password for the next level is stored in the file data.txt, which contains base64 encoded data

`bandit10`:`FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

    ssh bandit10@bandit.labs.overthewire.org -p 2220

<img width="598" height="103" alt="image" src="https://github.com/user-attachments/assets/748b8c06-09cc-4260-9795-aa3b338b8d29" />

Password for next level:

    dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

## Level 11 → Level 12

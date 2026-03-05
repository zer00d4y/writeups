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


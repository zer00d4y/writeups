# Cyber Espionage Write-up

Task Description:

Cyber Espionage

Points: 2500

OSINT

In the country of the Electropole, famous for its advanced technologies and advanced digital infrastructure, the cyber war with neighboring Cybergrad was not a sudden development. This conflict had a very long history, when both countries competed for dominance in the region.

The first steps in cyber warfare were subtle and hidden. Cyber espionage, attacks on critical infrastructure, propaganda - all this has become part of the cold cyber war between Electropoly and Cybergrad. Both sides used their best minds and resources to gain an advantage in the digital space.

In this conflict environment, special agents of the Electropole foreign intelligence played a key role. It is in this context that you, a special agent, were given a very important task from the military authorities: to find the email and password of Proton Mail of a very influential hacker of the cyber army of Cybergrad under the pseudonym b3nzino in order to compromise him. This hacker, known for his skills and influence, was a key player in the cyber strategy of the neighboring country. Don't let your people down!

Flag format: aitumilitaryctf{email_password}

--------------------------------------------------------------------------------------------------------

Seach `b3nzino` and found his github profile

![image](https://github.com/zer00d4y/writeups/assets/128820441/bdad799d-1dd7-4548-b4c6-fc6bd29433c3)

Look to `myfile` repository 

https://github.com/b3nzino/myfiles

![image](https://github.com/zer00d4y/writeups/assets/128820441/6a349b9a-b8fa-4d75-886d-f56c2e74d52a)

Look to address.zip

Archive protected with password, brute it using john 

![image](https://github.com/zer00d4y/writeups/assets/128820441/175d4e08-2e6d-48c3-9df3-be5329a99b3b)

![image](https://github.com/zer00d4y/writeups/assets/128820441/4f2b159e-d3b4-4d99-8658-2a75b140152a)

Password for archive is `Elle`

    ┌──(kali㉿kali)-[~]
    └─$ cat txt.txt    
    https://pastebin.com/Jxs0HAbL

Visit pastebin url

![image](https://github.com/zer00d4y/writeups/assets/128820441/4a1217be-eb7c-412a-b02a-e0c4f014f1b2)

    Up-to-date list of addresses
     
    My active address:
    bc1q4h6aszygl28tew2mnrrtr3smx9ffl0zd7t42fk
    bc1qfdtl7ga6z97thhm43fpfjh8c7x69syswfetmkg
     
    My addresses that are reported by people:
    bc1qxk6wqv9azrt5x79g5mz3nedwm2rc4qdknkwkf9

Here is bitcoin addresses 







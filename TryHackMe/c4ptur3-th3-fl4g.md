# c4ptur3-th3-fl4g Write-up

![image](https://tryhackme-images.s3.amazonaws.com/room-icons/8b906b3d444362152f1cd3e521aa7e4a.png)

room link: https://tryhackme.com/room/c4ptur3th3fl4g

## Translation & Shifting

1) c4n y0u c4p7u23 7h3 f149?

Just leet to text

answer: `can you capture the flag?`

2) 01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001

Binary to text

answer: `lets try some binary out!`

3) MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======

base32

answer: `base32 is super common in CTF's`

4) RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==

base64

answer: `Each Base64 digit represents exactly 6 bits of data.`

5) 68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f

hex to text

answer: `hexadecimal or base16?`

6) Ebgngr zr 13 cynprf!

ROT13

answer: `Rotate me 13 places!`

7) *@F DA:? >6 C:89E C@F?5 323J C:89E C@F?5 Wcf E:>6DX

ROT47

answer: `You spin me right round baby right round (47 times)`

8) - . .-.. . -.-. --- -- -- ..- -. .. -.-. .- - .. --- -. . -. -.-. --- -.. .. -. --.

morse code

answer: `telecommunication encoding`

9) 85 110 112 97 99 107 32 116 104 105 115 32 66 67 68

Binary Coded Decimal

answer: `Unpack this BCD`

10) LS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0g...

Base64->MorseCode->Binary->ROT47->Decimal

answer: `Let's make this a bit trickier...`

##  Spectrograms

This audio file is a spectrogram.
I'm using Audacity and spectrogram mode.

![image](https://github.com/zer00d4y/writeups/assets/128820441/e1e8431a-a3e3-4990-bcb4-af3295ee8c7c)

answer: `super secret message`

##  Steganography

Use online stegonagraphy tool 

![stegosteg](https://github.com/zer00d4y/writeups/assets/128820441/7fcccaba-942a-410a-951a-ad8f698e5b44)

answer: `SpaghettiSteg`

## Security through obscurity

Given an image, we need to open the image as an archive and inside there is another image. where will be flags

Or easy way is open like as text for example in terminal `cat meme.jpg` and scroll down, where will be flags

answer-1: `hackerchat.png`

answer-2: `AHH_YOU_FOUND_ME!`

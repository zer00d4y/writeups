# UofTCTF 2025 Write-up

University of Toronto CTF 2025

![image](https://ctftime.org/media/cache/f5/c5/f5c5229f1da4986f49cec241dcfbb13a.png)

CTFtime link: https://ctftime.org/event/2570

## WEB 

### WEB - Scavenger Hunt

part - 1

Source code

![image](https://github.com/user-attachments/assets/04dda408-b54b-42bb-8a86-21205bbaf5a9)

part - 2 

In response 

![image](https://github.com/user-attachments/assets/3d631887-23a7-4633-9ee5-bb60fabe150a)

part - 3

In Cookie 

![image](https://github.com/user-attachments/assets/4fd2dff8-e0e3-427d-9894-225eb9cf9546)

part - 4

In `/robots.txt`

![image](https://github.com/user-attachments/assets/19134bc7-f76d-42da-8c26-404efbbddea6)

part - 5 

In `style.css` file.

![image](https://github.com/user-attachments/assets/5cff560b-4b1e-42a9-b7fb-2e14c8088747)

part - 6

Intercept the request and change the role to `admin` in the cookie.

![image](https://github.com/user-attachments/assets/e52625e8-7a82-4a75-9227-340e20644d77)

part - 7

In source map file `/app.min.js.map`

![image](https://github.com/user-attachments/assets/58452e91-e018-4697-a19d-c716f95e1c17)

## Misc

### Misc - Surgery

I was thinking of getting some facial contouring plastic surgery done, but didn't know who to go to. My friend said they had a recommendation for a doctor specializing in that, but only sent me this photo of some building. Who's the doctor? Before submitting, wrap the doctor's name in uoftctf{}. Special characters allowed, [First] [Last] format.

For example, if the name was Jean-Pierre Brehier, the flag would be uoftctf{Jean-Pierre Brehier}

<img src="https://github.com/user-attachments/assets/6fdd0ce0-632d-4995-92dc-82b8a35d425f" width="350" height="350"> 

With a picture search, we found that the tower belongs to JK Plastic Surgery Clinic.
Go to the website and find the surgeon.

TEAM JK link: https://www.jkplastic.com/en/about-us/our-value/doctors.asp

<img src="https://github.com/user-attachments/assets/4ea0c69a-6971-4b9c-809c-b4c627b03d25" width="450" height="350"> 

FLAG:

    uoftctf{Sung-Sik Kim}

### Misc - Math Test

The server gives a flag if you solve all 1000 tasks correctly, by automating it with a script on python we can get a flag easily.

Use script below to connect the server and solve all 1000 math questions and get flag!

    import socket
    import re
    
    def solve_equation(equation):
        """Solve the given mathematical equation."""
        try:
            result = eval(equation)
            return int(result)  # Convert to integer if required by the server
        except Exception as e:
            print(f"Error solving equation: {equation}. Error: {e}")
            return None
    
    def main():
        host = '34.66.235.106'  # Server IP address
        port = 5000             # Server port
    
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
            try:
                s.connect((host, port))
                print("Connected to the server.")
    
                while True:
                    data = s.recv(4096).decode('utf-8')  # Increase buffer size
                    if not data:
                        print("Connection closed by server.")
                        break
                    
                    print(f"Received: {data.strip()}")
    
    
                    match = re.search(r'Question:\s*([^\n]+)', data)
                    if match:
                        equation = match.group(1).strip()  # Extract the equation
                        print(f"Solving equation: {equation}")
    
                        # Solve the equation
                        answer = solve_equation(equation)
                        if answer is not None:
                            print(f"Sending answer: {answer}")
                            s.sendall(f"{answer}\n".encode('utf-8'))
                        else:
                            print("Could not solve the equation. Exiting.")
                            break
                    else:
                        print("No valid equation found in the received data.")
                        break
            except Exception as e:
                print(f"Error: {e}")
            finally:
                print("Disconnected from the server.")
    
    if __name__ == "__main__":
        main()

![image](https://github.com/user-attachments/assets/a96b4a5d-9deb-454e-ae5d-a24f3e6af938)

FLAG:

    uoftctf{7h15_15_b451c_10_7357_d16u153d_45_4_m47h_7357}

### Misc - Sanity Check

We can find the flag in the discord channel of the event!

<img src="https://github.com/user-attachments/assets/b91025a4-9868-4fed-8cb3-bd6d33ef80c5" width="350" height="350"> 

FLAG:

    uoftctf{welcome_to_uoftctf_2025!!!!!}

## Crypto

### Crypto - Funny Cipher

A wheelbarrow ran over the flag. Can you fix it?

`60_ZMZ_GBWKNREM_KRA__LQM}WEPRGQL__Q_{RWW_M_KIAGPMNMRXDLM_FMWLDQ0BOIAMNPG`

look at the title and look for wheelbarrow cipher

Burrows–Wheeler Transform: https://www.dcode.fr/burrows-wheeler-transform

![image](https://github.com/user-attachments/assets/bb898f4f-84bd-443a-a56e-f81e56c45e3f)

LWPMGMP{NRN_XWL_BDWO_MZA_PRIQM_QLKQMRMLMRWD_GRFZAI_NEMAQ_KEGB_MW_600_KG}

![image](https://github.com/user-attachments/assets/0c950da5-7338-4f35-a2bd-6572b62dcf01)

FLAG:

	UOFTCTF{DID_YOU_KNOW_THE_FIRST_SUBSTITUTION_CIPHER_DATES_BACK_TO_600_BC}

## Forensics

### Forensics - Decrypt Me

`hashcat -m 13000 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

	$rar5$16$1d7cb8859a6c3c8e30a9db7a501811ac$15$280234db9d29c6ab216b74e6a89ec226$8$d12d4ba211b9c642:toronto416
	                                                          
	Session..........: hashcat
	Status...........: Cracked
	Hash.Mode........: 13000 (RAR5)
	Hash.Target......: $rar5$16$1d7cb8859a6c3c8e30a9db7a501811ac$15$280234...b9c642
	Time.Started.....: Tue Jan 14 01:35:07 2025 (3 hours, 6 mins)
	Time.Estimated...: Tue Jan 14 04:41:53 2025 (0 secs)
	Kernel.Feature...: Pure Kernel
	Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
	Guess.Queue......: 1/1 (100.00%)
	Speed.#1.........:      475 H/s (12.10ms) @ Accel:32 Loops:1024 Thr:1 Vec:8
	Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
	Progress.........: 3118272/14344385 (21.74%)
	Rejected.........: 0/3118272 (0.00%)
	Restore.Point....: 3118080/14344385 (21.74%)
	Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:32768-32799
	Candidate.Engine.: Device Generator
	Candidates.#1....: toroscolor3407 -> toroha
	Hardware.Mon.#1..: Util: 72%

FLAG:

	uoftctf{ads_and_aes_are_one_letter_apart}

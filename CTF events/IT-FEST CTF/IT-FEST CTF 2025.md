# IT-FEST CTF 2025 Write-up

## HARDWARE 

### Task - i2c

Just i2c and nothing more

    LOGIC ANALYZER CAPTURE: I2C BUS
    ADDR: 0x3C (Write Mode)
    --------------------------------
    [CMD] Control: 0x00, Data: 0xAE
    [CMD] Control: 0x00, Data: 0xD5
    [CMD] Control: 0x00, Data: 0x80
    [CMD] Control: 0x00, Data: 0xA8
    [CMD] Control: 0x00, Data: 0x3F
    [CMD] Control: 0x00, Data: 0xD3
    [CMD] Control: 0x00, Data: 0x00
    [CMD] Control: 0x00, Data: 0x40
    [CMD] Control: 0x00, Data: 0x8D
    [CMD] Control: 0x00, Data: 0x14
    [CMD] Control: 0x00, Data: 0x20
    [CMD] Control: 0x00, Data: 0x00
    [CMD] Control: 0x00, Data: 0xAF
    [CMD] Control: 0x00, Data: 0xB0 (Set Page Start)
    [CMD] Control: 0x00, Data: 0x00 (Set Lower Column)
    [CMD] Control: 0x00, Data: 0x10 (Set Higher Column)
    ---- croped for brevity ----

Solution: 

The I2C traffic targets address 0x3C (typical SSD1306 OLED controller) in write mode. 
Commands like 0xAE (display off), 0xD5/A8/3F (timing/multiplex), 0xA4 (normal display), 0x20 (horizontal addressing), and 0xAF (display on) initialize a 128x64 monochrome OLED. 
Page addressing (0xB0-B7) sets 8 display pages, each with column address 0x0010 (16-143 decimal) for 128 bytes of 8-bit vertical pixel data.

Script extracts DATA bytes (control 0x40) and reconstruct the 128x64 bitmap.

    data = []
    # Page B0 (mostly 0x00 - blank)
    data.extend([0x00] * 128)
    # Page B1 (mostly 0x00 - blank) 
    data.extend([0x00] * 128)
    # Page B2 (mostly 0x00 - blank)
    data.extend([0x00] * 128)
    # Page B3 (flag data starts here - key non-zero bytes)
    page_b3 = [0x00]*112 + [0x40,0xF0,0x50,0x00,0x20,0x10,0xF0,0x00,0x00,0x20,0x10,0x90,0x90,0x60,0x00,0x00,0xF8,0x08,0x40,0xF0,0x40,0x00,0xC0,0x00,0x00,0x00,0xC0,0x00,0x00,0xC0,0x40,0x40,0x00,0xC0,0xC0,0x40,0x40,0x80,0x00,0x00,0x00,0x00,0x00,0x00,0x80,0x40,0x40,0x40,0x80,0x00,0x40,0xF0,0x50,0x40,0xF0,0x50,0x00,0x00,0x00,0x00,0x00,0xC0,0x00,0x00,0x00,0xC0,0x00,0x80,0x40,0x40,0x40,0x80,0x00,0x00,0xC0,0x00,0x00,0x00,0xC0,0x00,0x00,0xC0,0x40,0x40,0x00,0x00,0x00,0x00,0x00,0x00,0x80,0x40,0x40,0xC0,0x00,0x00,0xD0,0x00,0x00,0xF0,0x00,0x08,0xF8,0x00,0x00,0x00]
    data.extend(page_b3)
    # Page B4 (some pattern data)
    page_b4 = [0x00]*13 + [0x0F,0x00,0x00,0x00,0x00,0x0F,0x00,0x00,0x06,0x08,0x08,0x08,0x07,0x00,0x00,0x1F,0x10,0x00,0x0F,0x08,0x00,0x07,0x08,0x08,0x0C,0x0F,0x00,0x00,0x0F,0x00,0x00,0x00,0x0F,0x00,0x00,0x00,0x0F,0x00,0x10,0x10,0x10,0x10,0x10,0x00,0x07,0x08,0x08,0x08,0x07,0x00,0x00,0x0F,0x00,0x00,0x0F,0x00,0x10,0x10,0x10,0x10,0x10,0x00,0x27,0x18,0x07,0x00,0x00,0x07,0x08,0x08,0x08,0x07,0x00,0x00,0x07,0x08,0x08,0x0C,0x0F,0x00,0x00,0x0F,0x00,0x00,0x10,0x10,0x10,0x10,0x10,0x00,0x0E,0x0A,0x0A,0x0F,0x00,0x00,0x0F,0x00,0x00,0x09,0x00,0x10,0x1F,0x00,0x00,0x00]
    data.extend(page_b4[:128])
    # Pages B5-B7 (all 0x00 - blank)
    data.extend([0x00] * 128 * 3)
    
    from PIL import Image
    img = Image.new('1', (128, 64), 0)
    pixels = img.load()
    
    for page in range(8):
        for col in range(128):
            byte = data[page*128 + col]
            for row in range(8):
                pixels[col, page*8 + row] = (byte >> (7-row)) & 1
    
    img = img.transpose(Image.FLIP_TOP_BOTTOM)  # OLED pages are top-to-bottom
    img.show()
    img.save('oled_flag.png')
    print("Flag image saved as oled_flag.png - check the image for flag text!")

<img width="400" height="250" alt="image" src="https://github.com/user-attachments/assets/3a563b20-762e-480a-9eec-1445c42f0d76" />

Output not good but it's readable, so get the flag!
 
FLAG:

    f13{turn_off_your_ai!}

# CRYPTO

## Task - XORSA

Solution:

    from sympy import mod_inverse
    
    # Given values 
    p = 8136275050578589354941365310845236830015617119134982422908833505524990840673975050206131066804435036075471735588017311510775351938363545669020267192173791
    q = 8136275050578589354941365310845236830015617119134982422908833505524990840673975050206131066804435036075471735588017311510775351938363545669020267192176083
    n = p * q
    c = 7909628388722349922026000796369904154754870043467924538646750692558839699959188311950225249912181943340447814258642765415584559383400716077797261462253392051177633213671341162600084678524736244177858359444505705375151780091622007189624527808666142433823575255166638317581931865191245934324356962633320716177
    e = 65537
    FLAG_LEN = 36
    
    # Decrypt RSA
    phi = (p - 1) * (q - 1)
    d = mod_inverse(e, phi)
    m = pow(c, d, n)
    byte_len = (m.bit_length() + 7) // 8
    m_bytes_full = m.to_bytes(byte_len, 'big')
    m_bytes = m_bytes_full[-FLAG_LEN:]  # Last 36 bytes
    
    # Brute-force keystream (RELAXED validation)
    print("m_bytes:", m_bytes.hex())
    for k0 in range(32, 256):
        ks = bytearray(32)
        ks[0] = k0
        for i in range(1, 32):
            ks[i] = (ks[i-1] * 9) % 256
        
        flag = bytes(m_bytes[i] ^ ks[i % 32] for i in range(FLAG_LEN))
        
        if (flag.startswith(b'f') and 
            b'}' in flag and 
            flag.count(b'_') >= 2):  # Look for underscores
            print(f"FLAG: f13{{{flag[3:].decode('ascii', errors='ignore')}}}")
            print(f"ks[0] = {k0}")
            break

FLAG:

    f13{f0und_XORed_byte5_and_fl4g_tabyldy!}

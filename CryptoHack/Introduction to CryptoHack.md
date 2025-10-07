# Introduction to CryptoHack

## Finding Flags

Solution:

Just read task description!

FLAG:

    crypto{y0ur_f1rst_fl4g}

## Great Snakes

Solution:

Run attached script and read flag!

    #!/usr/bin/env python3
    
    import sys
    # import this
    
    if sys.version_info.major == 2:
        print("You are running Python 2, which is no longer supported. Please update to Python 3.")
    
    ords = [81, 64, 75, 66, 70, 93, 73, 72, 1, 92, 109, 2, 84, 109, 66, 75, 70, 90, 2, 92, 79]
    
    print("Here is your flag:")
    print("".join(chr(o ^ 0x32) for o in ords))

FLAG:

    crypto{z3n_0f_pyth0n}

## ASCII

Solution:

    arr = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]
    flag = ''.join(chr(i) for i in arr)
    print(flag)

FLAG:

    crypto{ASCII_pr1nt4bl3}

## Hex

Solution:

    hex_string = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"
    flag_bytes = bytes.fromhex(hex_string)
    flag = flag_bytes.decode('ascii')
    print(flag)

FLAG:

    crypto{You_will_be_working_with_hex_strings_a_lot}

## Base64

Solution:

    import base64
    
    hex_string = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"
    byte_data = bytes.fromhex(hex_string)
    base64_encoded = base64.b64encode(byte_data).decode('ascii')
    
    print(base64_encoded)

FLAG:

    crypto/Base+64+Encoding+is+Web+Safe/

## Bytes and Big Integers

Solution:

    from Crypto.Util.number import long_to_bytes
    
    num = 11515195063862318899931685488813747395775516287289682636499965282714637259206269
    message_bytes = long_to_bytes(num)
    message = message_bytes.decode('ascii')
    
    print(message)

FLAG:

    crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}

## XOR Starter

Solution:

    def xor_string_with_key(s, key):
        return ''.join(chr(ord(c) ^ key) for c in s)
    
    input_string = "label"
    key = 13
    result = xor_string_with_key(input_string, key)
    flag = f"crypto{{{result}}}"
    print(flag)

FLAG:

    crypto{aloha}

## XOR Properties

Solution:

    from pwn import xor
    
    key1 = bytes.fromhex("a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313")
    key1_2 = "37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e"
    key2_3 = "c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1"
    flag_key123 = "04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf"
    
    key2 = xor(bytes.fromhex(key1_2), key1)
    key3 = xor(bytes.fromhex(key2_3), key2)
    
    key1_2_3 = xor(bytes.fromhex(key1_2), key3)
    
    flag = xor(bytes.fromhex(flag_key123), key1_2_3)
    
    print(f"Flag: {flag.decode(errors='ignore')}")
    print(f"Key1: {key1.hex()},\nKey2: {key2.hex()},\nKey3: {key3.hex()},\nKey1 ^ Key2 ^ Key3: {key1_2_3.hex()}")

FLAG:

    crypto{x0r_i5_ass0c1at1v3}

## Favourite byte

Solution:

FLAG:

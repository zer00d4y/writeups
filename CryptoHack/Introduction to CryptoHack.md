# Introduction to CryptoHack

## ASCII

    arr = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]
    flag = ''.join(chr(i) for i in arr)
    print(flag)

FLAG:

    crypto{ASCII_pr1nt4bl3}

## Hex

    hex_string = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"
    flag_bytes = bytes.fromhex(hex_string)
    flag = flag_bytes.decode('ascii')
    print(flag)

FLAG:

    crypto{You_will_be_working_with_hex_strings_a_lot}

## Base64

    import base64
    
    hex_string = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"
    byte_data = bytes.fromhex(hex_string)
    base64_encoded = base64.b64encode(byte_data).decode('ascii')
    
    print(base64_encoded)

FLAG:

    crypto/Base+64+Encoding+is+Web+Safe/

## Bytes and Big Integers

    from Crypto.Util.number import long_to_bytes
    
    num = 11515195063862318899931685488813747395775516287289682636499965282714637259206269
    message_bytes = long_to_bytes(num)
    message = message_bytes.decode('ascii')
    
    print(message)

FLAG:

    crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}

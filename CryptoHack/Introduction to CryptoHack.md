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

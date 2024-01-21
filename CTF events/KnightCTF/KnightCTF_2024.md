# KnightCTF 2024 Write-up

![image](https://ctftime.org/media/cache/7b/d5/7bd59748c9ed3a1b74eb82f4bcd85581.png)

## WEB

### Web - Levi Ackerman

Given web-site

![image](https://github.com/zer00d4y/writeups/assets/128820441/069e238c-33c7-43ec-ba7d-f14b2edb0e98)

Chech standart directory, like `robots.txt`

![image](https://github.com/zer00d4y/writeups/assets/128820441/99a9ed5b-da73-429c-bad2-133638d29e66)

Just visit `l3v1_4ck3rm4n.html`

![image](https://github.com/zer00d4y/writeups/assets/128820441/63f71d19-b205-4782-8df5-dbc2382b0f5d)

FLAG:

    KCTF{1m_d01n6_17_b3c4u53_1_h4v3_70}

### Web - Kitty

![image](https://github.com/zer00d4y/writeups/assets/128820441/2b7da697-db84-464c-b612-2faa21452dec)

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Login Page</title>
        <link rel="stylesheet" href="/static/style.css">
    </head>
    <body>
        <div class="container">
            <h2>Login</h2>
            <form id="login-form" action="/login" method="POST">
                <label for="username">Username</label>
                <input type="text" id="username" name="username" required>
                <label for="password">Password</label>
                <input type="password" id="password" name="password" required>
                <button type="submit">Login</button>
            </form>
        </div>
    
        <script src="/static/script.js"></script>
    </body>
    </html>

`script.js`

    document.getElementById('login-form').addEventListener('submit', function(event) {
        event.preventDefault();
    
        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;
    
        const data = {
            "username": username,
            "password": password
        };
    
        fetch('/login', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        })
        .then(response => response.json())
        .then(data => {
            // You can handle the response here as needed
            if (data.message === "Login successful!") {
                window.location.href = '/dashboard'; // Redirect to the dashboard
            } else {
                // Display an error message for invalid login
                const errorMessage = document.createElement('p');
                errorMessage.textContent = "Invalid username or password";
                document.getElementById('login-form').appendChild(errorMessage);
    
                // Remove the error message after 4 seconds
                setTimeout(() => {
                    errorMessage.remove();
                }, 4000);
            }
        })
        .catch(error => {
            console.error('Error:', error);
        });
    });
    
![image](https://github.com/zer00d4y/writeups/assets/128820441/df81345b-5cc5-4574-8b0b-b608cae1747c)

![image](https://github.com/zer00d4y/writeups/assets/128820441/10fa519e-f213-407d-aad5-3e020bc854cb)

FLAG:

    KCTF{Fram3S_n3vE9_L1e_4_toGEtH3R}

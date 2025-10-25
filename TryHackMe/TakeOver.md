# TakeOver Write-up

Add ip and domain to `/etc/hosts`

    echo "10.201.10.152 futurevera.thm" | sudo tee -a /etc/hosts

## Sub-domain enumeration via `FFUF`

    ffuf -w /usr/share/amass/wordlists/subdomains-top1mil-5000.txt -H "Host: FUZZ.futurevera.thm" -u https://10.201.10.152 -fs 4605

<img width="1149" height="464" alt="image" src="https://github.com/user-attachments/assets/c5f4cd7d-bcd0-4eb6-a8c1-7a44b78f2f2b" />

Now, we found `blog` and `support` subdomains! Add to `/etc/hosts`.

    echo "10.201.10.152 blog.futurevera.thm support.futurevera.thm" | sudo tee -a /etc/hosts

support.futurevera.thm

<img width="1279" height="743" alt="image" src="https://github.com/user-attachments/assets/88f22763-ce5f-4816-8570-c61d3bce6714" />

Check SSL certificate details!

<img width="741" height="579" alt="image" src="https://github.com/user-attachments/assets/18732907-7a37-4f9c-8652-c67735899a74" />

We can found another secret subdomain inside SSL description!

secrethelpdesk934752.support.futurevera.thm

<img width="1272" height="923" alt="image" src="https://github.com/user-attachments/assets/624c8cfa-d281-486a-b07a-866841f3dc03" />

Add to `/etc/hosts`!

    echo "10.201.122.171 secrethelpdesk934752.support.futurevera.thm" | sudo tee -a /etc/hosts

When we try to visit by http it's redirect us to https://flag{beea0d6edfcee06a59b83fb50ae81b2f}.s3-website-us-west-3.amazonaws.com/

So, link contains flag!

<img width="927" height="183" alt="image" src="https://github.com/user-attachments/assets/5e31a89a-2796-497d-9993-2d98f8f5ab99" />

FLAG:

    flag{beea0d6edfcee06a59b83fb50ae81b2f}

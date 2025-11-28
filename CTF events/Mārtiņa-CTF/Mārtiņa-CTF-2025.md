## Reverse

### Task - A Series of Tubes

<img width="600" height="900" alt="image" src="https://github.com/user-attachments/assets/59683c29-4949-4298-a236-9540b70a657f" />

Script:

    import hashlib
    import itertools
    
    charset = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789{}@/;?!_-"'
    
    target_hashes = [
        "edd94d6e72b217ef22de208d5246300c",
        "73a338a5cb6f23fb56cfd9320ea18414",
        "3e3e0a54a3a0bced0522f414db8df3aa",
        "665dc131f2c2104e7cba175cdb0f37d7",
        "6bc898592761e343f5284ee0e1a641be",
        "cf358237d3a6cbea098de5fb5b733f24",
        "c670e2c9053c8e9e600c8bc01ddd16ae",
        "010b08f101ec8929efff98e5e6418b7c",
        "e0bc06e650fd4a19f67951e6cf0b3615",
        "83d06450b3ad8b05ba802f0c117e706f",
        "32c24e443f8997e7eb14a212dccb707d",
        "54021079e528a133037e732a36774509"
    ]
    
    def brute_force_md5_joined(target_hashes):
        found_strings = []
        for target_hash in target_hashes:
            for combo in itertools.product(charset, repeat=3):
                candidate = ''.join(combo) + '\n'
                candidate_hash = hashlib.md5(candidate.encode('utf-8')).hexdigest()
                if candidate_hash == target_hash:
                    found_strings.append(''.join(combo))
                    print(f"Found for {target_hash}: {''.join(combo)}")
                    break
        joined_result = ''.join(found_strings)
        print(f"\nFLAG:\n{joined_result}")
        return joined_result
    
    brute_force_md5_joined(target_hashes)

<img width="420" height="297" alt="image" src="https://github.com/user-attachments/assets/39c42e90-35fc-4ef2-94e4-33a5dfd1ff75" />

FLAG:

    MCTF25{p1p1ng_h0t_t45k_y3t_n0_p1p15}

<img width="1532" height="715" alt="image" src="https://github.com/user-attachments/assets/bb4a7ed8-fd8a-45f7-a627-9482010518b9" />


<img width="1533" height="563" alt="image" src="https://github.com/user-attachments/assets/a8d5376d-896b-4849-b144-89c6a30a80ca" />

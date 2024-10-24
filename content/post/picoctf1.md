+++
title = 'Picoctf1'
date = 2024-10-24T12:15:29+03:00
draft = true
+++
+++
author = "Alex Domnit"
title = 'Custom Encryption'
date = 2024-10-24T10:51:20+03:00
draft = false
description = "Writeup"
tags = [
    "CTF",
    "Completed",
]
categories = [
    "PicoCTF",
    "Cryptography",
]
toc = false
+++

### Level Description

Can you get sense of this code file and write the function that will decode the given encrypted file content. Find the encrypted file here flag_info and code file might be good to analyze and get the flag. 

### Given Data

**enc_flag :**

```
a = 94
b = 21
cipher is: [131553, 993956, 964722, 1359381, 43851, 1169360, 950105, 321574, 1081658, 613914, 0, 1213211, 306957, 73085, 993956, 0, 321574, 1257062, 14617, 906254, 350808, 394659, 87702, 87702, 248489, 87702, 380042, 745467, 467744, 716233, 380042, 102319, 175404, 248489]
```

**custom_encryption.py :**

```
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')


if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")

```

### Solution :

After looking for way to long at the code and worrying about the fact that some of the values were randomly generated and didn`t know how I could reverse engineer that, I **catted** enc_flag and saw that I have the actual values used for the specific given encryption. The process from now on is pretty straight forward, we just need to define what we know, reverse each function and call the sequence for the given output.

#### What do we know?

```
# given cipher
cipher = [131553, 993956, 964722, 1359381, 43851, 1169360, 950105, 321574, 1081658, 613914, 0, 1213211, 306957, 73085, 993956, 0, 321574, 1257062, 14617, 906254, 350808, 394659, 87702, 87702, 248489, 87702, 380042, 745467, 467744, 716233, 380042, 102319, 175404, 248489]
# values
p = 97
g = 31
a = 94
b = 21
#generator function which we can reuse cause it doesn`t really matter the logic all that much
def generator(g, x, p):
    return pow(g, x) % p
#text_key
text_key="trudeau"
```

**Encrypt function :**

```
def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher
```

**Decrypt function :**

```
def decrypt(given_str, key):
    plaintext = ""
    for val in given_str:
        val = val // (key * 311)
        plaintext += chr(decrypted_value)  # just the reverse of ord(char)*key*311
    return plaintext
```

**Dynamic XOR encrypt :**

```
def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text
```


**Dynamic XOR decrypt :**

```
def dynamic_xor_decrypt(plaintext, text_key):
    # Initialize an empty string to hold the cipher text after each round of XOR
    cipher_text = ""
    # Calculate the length of the key to loop through it in a cyclical manner
    key_length = len(text_key)

    # First round of XOR decryption (reverse the string and XOR each character with the corresponding key character)
    for i, char in enumerate(plaintext[::-1]):  # Enumerate over the reversed plaintext
        key_char = text_key[i % key_length]     # Cycle through the key using modulus
        encrypted_char = chr(ord(char) ^ ord(key_char))  # XOR the ASCII values of the current char and key char
        cipher_text += encrypted_char           # Append the decrypted character to the result

    # Update plaintext to be the newly decrypted text from the first round
    plaintext = cipher_text
    # Reset cipher_text for the next round of decryption
    cipher_text = ""

    # Second round of XOR decryption (same process, but using the result from the first round)
    for i, char in enumerate(plaintext[::-1]):  # Reversing the text again
        key_char = text_key[i % key_length]     # Cycle through the key again
        encrypted_char = chr(ord(char) ^ ord(key_char))  # Perform XOR decryption again
        cipher_text += encrypted_char           # Append the decrypted character

    # Update plaintext to be the newly decrypted text from the second round
    plaintext = cipher_text
    # Reset cipher_text for the third and final round
    cipher_text = ""

    # Third round of XOR decryption (repeat the same process)
    for i, char in enumerate(plaintext[::-1]):  # Reverse again
        key_char = text_key[i % key_length]     # Cycle through the key
        encrypted_char = chr(ord(char) ^ ord(key_char))  # XOR decryption
        cipher_text += encrypted_char           # Append the decrypted character

    # Return the final decrypted result after three rounds of XOR decryption
    return cipher_text
```

Now all there`s left to do is calling the test function with the reversed functions and given values and watch the output.

### Result

```
11:16:49 archie@Archie pico → python3 decrypt.py
picoCTF{custom_d2cr0pt6d_8b41f976}
```
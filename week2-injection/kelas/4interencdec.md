# Interencdec

## Challenge Information

Level: Easy

Tags: picoCTF 2024, Cryptography, Base64, browser_webshell_solvable, Caesar

Author: NGIRIMANA SCHADRACK

Description:

“Can you get the real meaning from this file?”

## Challenge Link: https://play.picoctf.org/practice/challenge/418

Hints:

Engaging in various decoding processes is of utmost importance

## Solution

1. **Menginspeksi File**

File enc_flag berisi:

<img width="661" height="59" alt="cat" src="https://github.com/user-attachments/assets/a793d776-cbdd-4fef-acf9-9db66e5fca0d" />

```bash
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6ZzJhMnd6TW1zeWZRPT0nCg==
```

Akhiran = menunjukkan ini kemungkinan Base64.

2. **Base64 Decode Pertama**

```bash
cat enc_flag | base64 -d
```

<img width="860" height="74" alt="base64d" src="https://github.com/user-attachments/assets/41a890f3-833d-4f44-8c62-475787b9805f" />

Output:

```bash
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ=='
```

Masih berupa string Python bytes yang berisi Base64 lagi.

3. **Base64 Decode Kedua**

```bash
echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ==" | base64 -d
```

<img width="948" height="82" alt="echo" src="https://github.com/user-attachments/assets/540e69e4-ba6d-4f70-b48f-2d607d031203" />

Output:

```bash
wpjvJAM{jhlzhy_k3jy9wa3k_86kl32k2}
```

Sekarang terlihat seperti teks dengan cipher rotasi (Caesar/ROT-N).

4. **Brute force dengan Python**

Berikut kode:
```bash
import string

alphabet = string.ascii_lowercase
alpha_len = len(alphabet)

def shift(cipher_text, key):
    result = ''
    for c in cipher_text:
        if c.islower():
            result += alphabet[(alphabet.index(c) + key) % alpha_len]
        elif c.isupper():
            result += alphabet[(alphabet.index(c.lower()) + key) % alpha_len].upper()
        else:
            result += c
    return result

enc_data = 'wpjvJAM{jhlzhy_k3jy9wa3k_86kl32k2}'

for i in range(1, alpha_len+1):
    plain = shift(enc_data, i)
    if 'picoCTF' in plain:
        print("ROT-%02d: %s" % (i, plain))
```

<img width="958" height="935" alt="python" src="https://github.com/user-attachments/assets/d07cecbf-f3fe-46b3-a8e6-14228b399a96" />

Output:

```bash
ROT-19: picoCTF{<REDACTED>}
```

<img width="920" height="116" alt="VirtualBox_veru_25_09_2025_01_22_47" src="https://github.com/user-attachments/assets/2744c0af-ffea-4b41-b141-2970de465bac" />

Diketahui rotasi yang dipakai adalah ROT-19.

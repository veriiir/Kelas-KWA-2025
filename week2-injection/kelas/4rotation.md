# Rotation

## Challenge Information

Level: Medium

Tags: picoCTF 2023, Cryptography

Author: LOIC SHEMA

Description:

“You will find the flag after decrypting this file”

## Challenge Link: https://play.picoctf.org/practice/challenge/373

Hints:

Sometimes rotation is right

## Solution

1. **Analisis Awal**

File yang diberikan berisi teks terenkripsi. Hint “Sometimes rotation is right” menunjukkan bahwa cipher yang digunakan kemungkinan besar adalah ROT/ Caesar cipher.

<img width="492" height="66" alt="3cat" src="https://github.com/user-attachments/assets/2c5faac6-e161-43be-9d91-ceaafba9bc2a" />

2. **Python Solution**

Untuk pendekatan manual, buat skrip rotation.py:

```bash
#!/usr/bin/python3
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

# Baca flag terenkripsi
with open("encrypted.txt", 'r') as fh:
    enc_flag = fh.read().strip()

for i in range(1, alpha_len + 1):
    plain = shift(enc_flag, i)
    if 'picoCTF' in plain:
        print(f"ROT-{i:02d}: {plain}")
```

<img width="1920" height="945" alt="3python" src="https://github.com/user-attachments/assets/ff131221-5a81-47e5-a4cd-5b266701689d" />

Jalankan skripnya:
```bash
chmod +x rotation.py
./rotation.py
```

Output:
```bash
ROT-18: picoCTF{<REDACTED>}
```

<img width="516" height="109" alt="3hasil" src="https://github.com/user-attachments/assets/4c9e5084-18b3-4ff7-8123-dc67fc0aa74e" />

Berhasil mendapatkan flag dengan rotasi 18.

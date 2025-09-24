# Custom Encryption

## Challenge Information

Level: Easy

Tags: picoCTF 2024, Cryptography, browser_webshell_solvable, ASCII_encoding, XOR

Author: NGIRIMANA SCHADRACK

Description:

“Can you get sense of this code file and write the function that will decode the given encrypted file content?”

## Challenge Link: https://play.picoctf.org/practice/challenge/412

Hints:

Understanding the encryption algorithm to come up with decryption algorithm.

## Solution

1. **Analisis Kode & Data**

File enc_flag berisi:
```bash
a = 94
b = 29
cipher is: [260307, 491691, 491691, 2487378, 2516301, 0, 1966764, 1879995, 1995687, 1214766, 0, 2400609, 607383, 144615, 1966764, 0, 636306, 2487378, 28923, 1793226, 694152, 780921, 173538, 173538, 491691, 173538, 751998, 1475073, 925536, 1417227, 751998, 202461, 347076, 491691]
```

<img width="1898" height="165" alt="2cat" src="https://github.com/user-attachments/assets/40278899-306b-4c71-b52b-d1a2d8eabcd1" />

File Python yang disediakan menunjukkan tiga langkah enkripsi:

Reversal + XOR: fungsi dynamic_xor_encrypt membalik string kemudian XOR per karakter dengan kunci teks.

Multiplikasi ASCII: fungsi encrypt mengalikan nilai ASCII hasil XOR dengan shared_key * 311.

2. **Strategi Dekripsi**

Proses dibalik secara urut:

Bagi setiap angka pada cipher dengan shared_key * 311 untuk dapatkan karakter XOR-ed.

XOR lagi dengan kunci teks untuk balikkan XOR (XOR adalah involutif).

Balik kembali urutan karakter (karena pada enkripsi dibalik).

3. **Skrip Dekripsi**
```bash
#!/usr/bin/python3

a = 94
b = 29
cipher = [260307, 491691, 491691, 2487378, 2516301, 0, 1966764, 1879995,
          1995687, 1214766, 0, 2400609, 607383, 144615, 1966764, 0, 636306,
          2487378, 28923, 1793226, 694152, 780921, 173538, 173538, 491691,
          173538, 751998, 1475073, 925536, 1417227, 751998, 202461, 347076, 491691]

text_key = "trudeau"

def generator(g, x, p):
    return pow(g, x) % p

def decrypt(ciphertext, key):
    return [chr(int(num / key / 311)) for num in ciphertext]

def dynamic_xor_decrypt(ciphertext, key):
    text = ""
    key_length = len(key)
    for i, char in enumerate(ciphertext):
        key_char = key[i % key_length]
        text += chr(ord(char) ^ ord(key_char))
    return text

if __name__ == "__main__":
    p = 97
    g = 31
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    if key != b_key:
        print("Invalid key")
        exit(1)

    semi_plain = decrypt(cipher, key)
    flag = dynamic_xor_decrypt(semi_plain, text_key)
    print(flag[::-1])
```

<img width="1920" height="945" alt="2python" src="https://github.com/user-attachments/assets/c0d74828-f154-41be-9ccb-ca6bc127e8a4" />

4. **Menjalankan Skrip**

```bash
./custom.py
```

Output:
```bash
picoCTF{<REDACTED>}
```

<img width="944" height="113" alt="hasil2" src="https://github.com/user-attachments/assets/91bf644b-b7a8-4d54-be1d-bdf13b4b1bbc" />

Berhasil memperoleh flag.

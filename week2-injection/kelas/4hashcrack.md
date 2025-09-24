# Hashcrack

## Challenge Information

Level: Easy

Tags: Cryptography, picoCTF 2025, browser_webshell_solvable

Author: Nana Ama Atombo-Sackey

Description:

“A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?”

## Challenge Link: https://play.picoctf.org/practice/challenge/475

Hints:

Understanding hashes is very crucial.

Identify the hash algorithm by its length and structure.

Try using hash-cracking tools.

## Solution

1. **Connect to the Server**

Gunakan netcat untuk berinteraksi:

nc verbal-sleep.picoctf.net 58372

<img width="984" height="120" alt="51" src="https://github.com/user-attachments/assets/7ea21df6-6e58-4b26-9946-f116cbfec5e8" />

Server akan menampilkan hash yang harus kita pecahkan.

2. **Crack Hash Pertama (MD5)**

Server memberikan:
```bash
482c811da5d5b4bc6d497ffa98491e38
```

Panjang 32 karakter → MD5. Cek di CrackStation
 → hasilnya password123.

<img width="1323" height="192" alt="52" src="https://github.com/user-attachments/assets/af513c38-2c3c-432d-a3a9-e8d6a4608c28" />

3. Crack Hash Kedua (SHA-1)

Server memberikan:
```bash
b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
```

Panjang 40 karakter → SHA-1. Cek di CrackStation → hasilnya letmein.

<img width="1082" height="271" alt="53" src="https://github.com/user-attachments/assets/1062a840-3291-4420-ae9d-0e10c44f6a60" />

4. Crack Hash Ketiga (SHA-256)

Server memberikan:
```bash
916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
```

Panjang 64 karakter → SHA-256. Cek di CrackStation → hasilnya qwerty098.

<img width="1004" height="271" alt="54" src="https://github.com/user-attachments/assets/42d540b9-c397-479c-8e5d-832346649333" />

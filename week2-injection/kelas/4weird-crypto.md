# Write-up: Weird Crypto

## Overview

Judul: Weird Crypto

Kategori: Cryptographic Issues

Tantangan “Weird Crypto” menguji kemampuan mengidentifikasi dan melaporkan penggunaan metode kriptografi yang sudah usang dalam aplikasi. Fokusnya adalah pada implementasi hashing kata sandi yang tidak aman.

<img width="436" height="207" alt="Cuplikan layar 2025-09-24 183744" src="https://github.com/user-attachments/assets/ce0ea508-edbd-4486-9045-20c20f20d0ed" />

## Alat yang Digunakan

**Web Browser**: Untuk memeriksa token autentikasi (JWT) dan mengirim laporan kerentanan.

**Base64 Decoder**: Untuk mendekode payload token yang di-encode Base64.

## Metodologi dan Solusi

1. **Analisis Token Autentikasi**

Inspeksi Token:

Ekstrak JWT (JSON Web Token) dari cookie browser.

Perhatikan bahwa payload JWT di-encode dalam Base64.

<img width="1198" height="822" alt="Cuplikan layar 2025-09-24 181135" src="https://github.com/user-attachments/assets/d4b57743-2129-473d-a027-33dcf56781a7" />

Gunakan Base64 decoder (atau JWT.io) untuk melihat isi payload.

2. **Mengidentifikasi Kelemahan Kriptografi**

Dekode Payload JWT:

Payload hasil dekode memperlihatkan atribut user termasuk password hash:

```bash
"password":"0192023a7bbd73250516f069df18b500"
```

Pola hash ini dikenali sebagai MD5, algoritma hashing yang sudah deprecated.

Analisis Hash:

MD5 sudah lama dianggap tidak aman karena rentan collision dan brute-force.

Identifikasi penggunaan MD5 sebagai inti dari kerentanan yang harus dilaporkan.

3. **Melaporkan Masalah**

Gunakan formulir Contact di aplikasi untuk melaporkan penggunaan MD5 sebagai hashing password yang tidak aman.

<img width="1897" height="961" alt="Cuplikan layar 2025-09-24 181411" src="https://github.com/user-attachments/assets/ed38b56a-3709-4a46-aabe-08dea8c216ef" />

## Penjelasan Solusi

Tantangan berhasil diselesaikan dengan mengidentifikasi penggunaan metode hashing yang usang (MD5). Dengan mendekode JWT dan menganalisis hash, kelemahan kriptografi terungkap dan dilaporkan melalui kanal resmi di aplikasi.

## Rekomendasi Perbaikan

Untuk mengurangi risiko yang timbul akibat penggunaan fungsi kriptografi yang deprecated:

Ganti Algoritma Hashing: Gunakan algoritma yang lebih kuat seperti SHA-256, bcrypt, scrypt, atau Argon2 yang tahan brute force.

Tambahkan Salt: Gunakan salt unik untuk setiap hash password guna mencegah penggunaan rainbow tables dan meningkatkan keamanan.

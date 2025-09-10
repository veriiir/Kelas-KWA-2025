# Juice-Shop Write-up: Login Jim

## Overview

Title: Login Jim

Category: SQL Injection

Decribe: Log in with Jim's user account.

<img width="427" height="229" alt="Cuplikan layar 2025-09-10 133005" src="https://github.com/user-attachments/assets/251d73d9-786b-491e-9d27-13d93e8afad0" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

### Resource

#### Payload : [sql payload list](https://github.com/payloadbox/sql-injection-payload-list)

### Metodologi dan Solusi

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Mencari Email milik Jim**
   
   Sebelum memulai pengerjaan, pertama kita mencari user bernama jim pada bagian review produk. User Jim terletak di review produk Green Smoothie.


<img width="1905" height="965" alt="Cuplikan layar 2025-09-10 134432" src="https://github.com/user-attachments/assets/9617dcb4-fb25-4cc5-bab3-afc1ca62127c" />

```bash
jim@juice-sh.op
```

2. **Masuk Halaman Login**
   
   Untuk masuk halaman login dapat mengklik kata account dibagian kanan atas.
   
<img width="1919" height="972" alt="Cuplikan layar 2025-09-10 134607" src="https://github.com/user-attachments/assets/97e0f05c-cc6f-422f-9334-de7c6ac8add0" />

4. **Uji SQL Injection pada Login Page**
   
   Untuk Payload SQL Injection bisa diliat di resorce yang sudah saya sertakan diatas.

```bash
' OR 1=1--
```
Untuk kolom **Password**, isi dengan sembarang nilai (contoh: akuhebat).

<img width="1919" height="986" alt="Cuplikan layar 2025-09-10 141603" src="https://github.com/user-attachments/assets/36f068f5-cf54-465b-8847-a860bd9871c2" />

6. **Hasil Login**
   
   Setelah menekan tombol Log in, sistem berhasil masuk sebagai Jim tanpa mengetahui password asli.
<img width="1919" height="962" alt="Cuplikan layar 2025-09-10 141650" src="https://github.com/user-attachments/assets/29d39b96-578c-4c76-bfdc-39029534e812" />

---

### Burpsuit

Kueri SQL yang Dieksploitasi

Kueri Asli:

```bash
SELECT * FROM users WHERE email = '[input_email]' AND password = '[input_password]'
```

Kueri yang Dieksploitasi:

```bash
SELECT * FROM users WHERE email = 'jim@juice-sh.op'--' AND password = 'veri'
```

Setelah email **jim@juice-sh.op**, ditambahkan comment setelah email yang mengabaikan password sehingga bisa login sebagai user Jim meskipun passwordnya salah.

Untuk soal ini juga bisa dikerjakan dengan Burp Suite untuk memodifikasi request. Tetapi karena langkah - langkahnya yang cukup mudah, soal ini bisa dikerjakan meski tidak menggunakan Burp Suite maupun tools lainnya.
<img width="1919" height="1019" alt="Cuplikan layar 2025-09-10 140939" src="https://github.com/user-attachments/assets/677f7df2-2783-4399-807a-2c801a7a2ef4" />
<img width="1919" height="1010" alt="Cuplikan layar 2025-09-10 141116" src="https://github.com/user-attachments/assets/6f6e400f-34e8-45e0-babf-57342faf93bb" />

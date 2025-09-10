# Juice-Shop Write-up: Login Bender

## Overview

Title: Login Bender

Category: SQL Injection

Decribe: Log in with Bender's user account.

<img width="426" height="213" alt="Cuplikan layar 2025-09-10 143811" src="https://github.com/user-attachments/assets/6cf2cd21-78a1-4d62-896e-ede9fc6baca6" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

### Resource

#### Payload : [sql payload list](https://github.com/payloadbox/sql-injection-payload-list)

### Metodologi dan Solusi

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Mencari Email milik Bender**
   
   Sebelum memulai pengerjaan, pertama kita mencari user bernama bender pada bagian review produk. User bender terletak di review produk Banana Juice.
<img width="1917" height="949" alt="Cuplikan layar 2025-09-10 142459" src="https://github.com/user-attachments/assets/b0ccd0d5-6d76-4e3c-8e86-75a5b5e398f8" />

```bash
bender@juice-sh.op
```

2. **Masuk Halaman Login**
   
   Untuk masuk halaman login dapat mengklik kata account dibagian kanan atas.
<img width="1919" height="972" alt="Cuplikan layar 2025-09-10 134607" src="https://github.com/user-attachments/assets/d10edc7a-999c-42e8-8235-8f8e539396a3" />

4. **Uji SQL Injection pada Login Page**
   
   Untuk Payload SQL Injection bisa diliat di resorce yang sudah saya sertakan diatas.
```bash
' OR 1=1--
```
Untuk kolom **Password**, isi dengan sembarang nilai (contoh: akuhebat).
<img width="1917" height="976" alt="Cuplikan layar 2025-09-10 142601" src="https://github.com/user-attachments/assets/b1dc56ce-334c-43f6-a312-63f3bf4a70d6" />

6. **Hasil Login**
   
   Setelah menekan tombol Log in, sistem berhasil masuk sebagai bender tanpa mengetahui password asli.
<img width="1916" height="979" alt="Cuplikan layar 2025-09-10 142632" src="https://github.com/user-attachments/assets/9760a800-6d26-4fe2-ac77-2a6d7316a938" />

---

### Burpsuit

Kueri SQL yang Dieksploitasi

Kueri Asli:

```bash
SELECT * FROM users WHERE email = '[input_email]' AND password = '[input_password]'
```

Kueri yang Dieksploitasi:

```bash
SELECT * FROM users WHERE email = 'bender@juice-sh.op'--' AND password = 'veri'
```

Setelah email **bender@juice-sh.op**, ditambahkan comment setelah email yang mengabaikan password sehingga bisa login sebagai user bender meskipun passwordnya salah.

Untuk soal ini juga bisa dikerjakan dengan Burp Suite untuk memodifikasi request. Tetapi karena langkah - langkahnya yang cukup mudah, soal ini bisa dikerjakan meski tidak menggunakan Burp Suite maupun tools lainnya.

<img width="1919" height="982" alt="Cuplikan layar 2025-09-10 143103" src="https://github.com/user-attachments/assets/ddca70fa-2108-44c3-bf04-9dd81ad46370" />
<img width="1919" height="964" alt="Cuplikan layar 2025-09-10 142940" src="https://github.com/user-attachments/assets/8c8eb330-e6fe-479d-b948-0c7eb1d32697" />

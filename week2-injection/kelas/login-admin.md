# Juice-Shop Write-up: Login Admin

## Overview

Title: Login Admin

Category: SQL Injection

Decribe: Log in with the administrator's user account.

<img width="433" height="241" alt="Cuplikan layar 2025-09-10 072546" src="https://github.com/user-attachments/assets/d67155d1-b0a4-4237-b909-bda02dc69a60" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

### Resource

## Payload : [sql payload list](https://github.com/payloadbox/sql-injection-payload-list)

### Metodologi dan Solusi

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Login**
   
   Untuk masuk halaman login dapat mengklik kata account dibagian kanan atas.
   
<img width="1915" height="967" alt="Cuplikan layar 2025-09-10 120004" src="https://github.com/user-attachments/assets/53d23d4e-3e3b-4c8e-a32e-9453623c55b6" />

2. **Uji SQL Injection pada kolom Email**
   
   Untuk Payload SQL Injection bisa diliat di resorce yang sudah saya sertakan diatas.

```bash
' OR 1 -- -
```

Untuk kolom **Password**, isi dengan sembarang nilai (contoh: akuhebat).
<img width="1918" height="967" alt="Cuplikan layar 2025-09-10 123741" src="https://github.com/user-attachments/assets/172e4192-8f6e-49c9-80e8-2519929eee90" />


     **Mencegat Permintaan Login:** Menggunakan Burp Suite, permintaan **HTTP POST** yang dikirim saat mencoba login dicegat. Permintaan ini berisi kredensial pengguna dalam format **JSON**.
     **Mengubah Kueri SQL:** Bidang email dalam muatan JSON diubah menjadi **`admin@juice-sh.op' ' OR 1 -- -**. Kode ini secara efektif mengubah perintah SQL menjadi pernyataan yang **selalu bernilai benar**, sehingga tidak lagi memerlukan kata sandi.
     **Menjalankan Permintaan yang Disuntikkan:** Permintaan yang telah dimodifikasi dikirim ke server. Karena kode SQL yang disuntikkan, kondisi **' OR 1 -- -** akan selalu dievaluasi sebagai **true**. Hal ini memungkinkan akses tanpa izin ke akun administrator.

<img width="1919" height="1019" alt="Cuplikan layar 2025-09-10 123354" src="https://github.com/user-attachments/assets/8b386d68-ef6f-4a3d-837c-ef517b4f55b2" />
     
<img width="1919" height="1019" alt="Cuplikan layar 2025-09-10 123354" src="https://github.com/user-attachments/assets/b837963d-f030-4619-9231-29ce317e055e" />

3. **Hasil Login**
   
   Setelah menekan tombol Log in, sistem berhasil mengautentikasi tanpa memverifikasi password asli administrator. Kita berhasil login sebagai admin.
   
<img width="1899" height="968" alt="Cuplikan layar 2025-09-10 123533" src="https://github.com/user-attachments/assets/b507ee62-a64c-4bf7-843d-36134a09f226" />

---

### Analisis Teknis

Kueri SQL yang Dieksploitasi

Kueri Asli:
```bash
SELECT * FROM users WHERE email = '[input_email]' AND password = '[input_password]'
```

Kueri yang Dieksploitasi:
```bash
SELECT * FROM users WHERE email = 'admin@juice-sh.op' OR 1 -- -' AND password = 'veri'
```

Karena kondisi ' OR 1 -- - selalu benar, database akan mengembalikan baris pertama dari tabel users, yang biasanya adalah akun administrator, sehingga berhasil melewati autentikasi.

### Penjelasan Solusi

Serangan **SQL Injection** ini berhasil karena adanya **kelemahan dalam validasi masukan** dan **kurangnya penggunaan _prepared statement_** saat menangani kueri SQL. Kode yang disuntikkan berhasil mengubah struktur logis dari perintah SQL, sehingga melewati proses otentikasi dan masuk sebagai administrator tanpa memerlukan kata sandi yang benar.

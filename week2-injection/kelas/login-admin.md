# Juice-Shop Write-up: Login Admin

## Overview

Title: Login Admin
Category: SQL Injection
Decribe: Log in with the administrator's user account.

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

### Resource

## Payload : [sql payload list](https://github.com/payloadbox/sql-injection-payload-list)

### Metodologi dan Solusi

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Login**
   Untuk masuk halaman login dapat mengklik kata account dibagian kanan atas.

2. **Uji SQL Injection pada kolom Email**
   Untuk Payload SQL Injection bisa diliat di resorce yang sudah saya sertakan diatas.

```bash
' OR 1 -- -
```

Untuk kolom **Password**, isi dengan sembarang nilai (contoh: akuhebat).

     **Mencegat Permintaan Login:** Menggunakan Burp Suite, permintaan **HTTP POST** yang dikirim saat mencoba login dicegat. Permintaan ini berisi kredensial pengguna dalam format **JSON**.
     **Mengubah Kueri SQL:** Bidang email dalam muatan JSON diubah menjadi **`admin@juice-sh.op' ' OR 1 -- -**. Kode ini secara efektif mengubah perintah SQL menjadi pernyataan yang **selalu bernilai benar**, sehingga tidak lagi memerlukan kata sandi.
     **Menjalankan Permintaan yang Disuntikkan:** Permintaan yang telah dimodifikasi dikirim ke server. Karena kode SQL yang disuntikkan, kondisi **' OR 1 -- -** akan selalu dievaluasi sebagai **true**. Hal ini memungkinkan akses tanpa izin ke akun administrator.

6. **Hasil Login**
   Setelah menekan tombol Log in, sistem berhasil mengautentikasi tanpa memverifikasi password asli administrator. Kita berhasil login sebagai admin.

---

### Penjelasan Solusi

Serangan **SQL Injection** ini berhasil karena adanya **kelemahan dalam validasi masukan** dan **kurangnya penggunaan _prepared statement_** saat menangani kueri SQL. Kode yang disuntikkan berhasil mengubah struktur logis dari perintah SQL, sehingga melewati proses otentikasi dan masuk sebagai administrator tanpa memerlukan kata sandi yang benar.

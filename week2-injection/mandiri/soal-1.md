# Write-up: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

## Overview

Title: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

Category: SQL Injection

Decribe: contains a SQL injection vulnerability in the product category filter

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)

Payloads: [sql payload list](https://github.com/payloadbox/sql-injection-payload-list)

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**
2. 
    <img width="1919" height="965" alt="Cuplikan layar 2025-09-10 211857" src="https://github.com/user-attachments/assets/c0b50165-8029-4965-bddf-cc4d045045e9" />

3. **Setelah masuk ke halaman login. Masukkan payload bebas pada url halaman web**

   _Note_: Disini bisa menggunakan burpsuit untuk memasukkan payloadnya, tetapi untuk lebih mudahnya bisa langsung di urlnya

   Sebelum memasukkan payload gunakan **filter?category=**

   Untuk Payload SQL Injection bisa diliat di resorce yang sudah saya sertakan diatas. Disini saya menggunakan payload seperti berikut:

   ```bash
   ' OR 1=1--
   ```
   <img width="1919" height="962" alt="Cuplikan layar 2025-09-10 213443" src="https://github.com/user-attachments/assets/0e97c855-2b27-4b26-9bc8-0d808b2bc582" />

6. **Hasil Injection**

   Setelah memasukan *filter?category='+OR+1=1--*', sistem berhasil mengautentikasi payload dan menyortir kategorinya.

   <img width="1919" height="1019" alt="Cuplikan layar 2025-09-10 211929" src="https://github.com/user-attachments/assets/d766dc64-2a7f-48eb-abc7-04ba490487c0" />

---

### Penjelasan Solusi

Serangan **SQL Injection** ini berhasil karena adanya **kelemahan dalam validasi masukan** dan **kurangnya penggunaan _prepared statement_** saat menangani kueri SQL. Kode yang disuntikkan berhasil mengubah struktur logis dari perintah SQL, sehingga dapat menampilkan data tersembunyi dari produk-produk.


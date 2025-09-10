# Write-up: SQL injection UNION attack, retrieving data from other tables

## Overview

Title: SQL injection UNION attack, retrieving data from other tables

Category: SQL Injection

Decribe: The database contains a different table called users, with columns called username and password. To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**
   
   <img width="1916" height="971" alt="Cuplikan layar 2025-09-10 231810" src="https://github.com/user-attachments/assets/ab54bec9-f02a-4040-9df8-88f5048619cc" />

3. **Setelah masuk ke halaman login. Masukkan payload bebas pada url halaman web**

   _Note_: Disini bisa menggunakan burpsuit untuk memasukkan payloadnya, tetapi untuk lebih mudahnya bisa langsung di urlnya

   Sebelum memasukkan payload gunakan **filter?category=**

   Disini saya menggunakan payload seperti berikut:

   ```bash
   '+UNION+SELECT+'a','a'--
   ```
   
   <img width="1914" height="964" alt="Cuplikan layar 2025-09-10 231951" src="https://github.com/user-attachments/assets/5cb4afe3-1c0f-4939-adbe-420cc90c1bd7" />

4. **Setelah verifikasi kolom query SQL, dapatkan kolom username dan password**

   tahap ini bisa diuji dengan payload:

   ```bash
   '+UNION+SELECT+username,password+FROM+users--
   ```
   <img width="1919" height="966" alt="Cuplikan layar 2025-09-10 232222" src="https://github.com/user-attachments/assets/d763205d-56d2-48cc-be46-284afbe290e8" />

   Username dan Password berhasil didapatkan
  
5. **Login sebagai user administrator dengan credential yang didapatkan.**

  <img width="1917" height="965" alt="Cuplikan layar 2025-09-10 232741" src="https://github.com/user-attachments/assets/94813c9d-2c7a-4986-bdba-d80ac7fc2859" />

### Catatan

Di soal ini, karena payload sudah pasti menggunakan dua kolom username dan password dan UNION membutuhkan tabel yang digabungkan untuk mempunyai jumlah kolom yang sama, ada baiknya untuk melakukan verifikasi terlebih dahulu. Hal ini karena bisa saja query SQL menggunakan 3 kolom, yang artinya untuk payload yang digunakan akan menggunakan kolom username, password, dan NULL. Karena di sini hanya ada dua kolom saja, maka tidak perlu NULL sebagai kolom tambahan.

# Write-up: SQL injection attack, querying the database type and version on Oracle

## Overview

Title: SQL injection attack, querying the database type and version on Oracle

Category: SQL Injection

Decribe: Make the database retrieve the strings: 'Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production, PL/SQL Release 11.2.0.2.0 - Production, CORE 11.2.0.2.0 Production, TNS for Linux: Version 11.2.0.2.0 - Production, NLSRTL Version 11.2.0.2.0 - Production'

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**
   
   <img width="1916" height="965" alt="Cuplikan layar 2025-09-10 225423" src="https://github.com/user-attachments/assets/9f346af4-4728-448b-b6bd-c46d667ca999" />
     
3. **Setelah masuk ke halaman login. Masukkan payload bebas pada url halaman web**

   _Note_: Disini bisa menggunakan burpsuit untuk memasukkan payloadnya, tetapi untuk lebih mudahnya bisa langsung di urlnya

   Sebelum memasukkan payload gunakan **filter?category=Gifts**

   Untuk Payload SQL Injection bisa diliat di resorce yang sudah saya sertakan diatas. Disini saya menggunakan payload seperti berikut:

   ```bash
   '+UNION+SELECT+NULL,NULL--
   ```
   
   <img width="1919" height="971" alt="Cuplikan layar 2025-09-10 225616" src="https://github.com/user-attachments/assets/22e709c0-b115-4dab-99c0-a4913923e2ee" />

   Kita bisa lihat hint pada halaman utama agar tau paylaod yang akan digunakan
   
   <img width="1051" height="331" alt="Cuplikan layar 2025-09-10 225300" src="https://github.com/user-attachments/assets/d2be5d2f-a6a8-4e8d-ae5a-b99430fadf3d" />

   Payload ini masih error, maka kita ubah payload menjadi seperti ini

   ```bash
   '+UNION+SELECT+NULL,NULL+FROM+dual--
   ```
   
   <img width="1915" height="965" alt="Cuplikan layar 2025-09-10 225934" src="https://github.com/user-attachments/assets/2a19524b-ea1e-4182-b634-ce4712e6c71e" />

5. **Uji coba tampilan type dan versi database**
   
    tahap ini bisa diuji dengan payload:
    ```bash
    '+UNION+SELECT+V,R+FROM+dual--
    ```
    
    <img width="1916" height="970" alt="Cuplikan layar 2025-09-10 230334" src="https://github.com/user-attachments/assets/41c1ac16-3ac7-47e4-a4a0-07c570aec62d" />

7. **Tampilkan Database Type dan Versi nya**

   Untuk hint payload bisa diliat di cheatsheet berikut: https://portswigger.net/web-security/sql-injection/cheat-sheet

   Payload yang digunakkan:
   
   <img width="1915" height="916" alt="Cuplikan layar 2025-09-10 222351" src="https://github.com/user-attachments/assets/6f4c24b4-6f9d-48f2-a057-398710d9d50b" />

   ```bash
   '+UNION+SELECT+Banner,NULL+FROM+v$version--
   ```
   
   <img width="1919" height="965" alt="Cuplikan layar 2025-09-10 230804" src="https://github.com/user-attachments/assets/1f7a529a-dc3d-446b-87eb-9932fe27c4ab" />

   Pada bagian bawah tertera Versi nya.

---

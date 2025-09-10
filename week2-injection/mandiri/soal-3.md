# Write-up: SQL injection attack, querying the database type and version on MySQL and Microsoft

## Overview

Title: SQL injection attack, querying the database type and version on MySQL and Microsoft

Category: SQL Injection

Decribe: contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL. (*Note: Conditional*)

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft) 

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**
   
   <img width="1918" height="969" alt="Cuplikan layar 2025-09-10 222446" src="https://github.com/user-attachments/assets/c26223fa-bd5c-430e-b837-80dc6e34f927" />

2. **Setelah masuk ke halaman login. Masukkan payload bebas pada url halaman web**

   _Note_: Disini bisa menggunakan burpsuit untuk memasukkan payloadnya, tetapi untuk lebih mudahnya bisa langsung di urlnya

   Sebelum memasukkan payload gunakan **filter?category=Pets**

    *Note: Kenapa Pets karena dari semua kategory itu yang paling pendek*
   
   Untuk Payload SQL Injection bisa diliat di cheatsheet yang sudah saya sertakan dibawah. Disini saya menggunakan payload seperti berikut:

   ```bash
   '+UNION+SELECT+NULL,NULL--
   ```
   
   <img width="1918" height="965" alt="Cuplikan layar 2025-09-10 222823" src="https://github.com/user-attachments/assets/ec70e623-0f98-45c7-83b3-86598dc19469" />

    Payload ini masih error, maka kita ubah payload menjadi seperti ini

    ```bash
    '+UNION+SELECT+NULL,NULL--+
    ```
    
    <img width="1919" height="974" alt="Cuplikan layar 2025-09-10 222936" src="https://github.com/user-attachments/assets/1f343738-aa51-4571-a332-3f7631c7eabc" />

4. **Tampilkan Database Type dan Versi nya**

    Untuk hint payload bisa diliat di cheatsheet berikut: https://portswigger.net/web-security/sql-injection/cheat-sheet

    Berikut Database Version dari Microsoft dan MYSQL:
   
    <img width="1915" height="916" alt="Cuplikan layar 2025-09-10 222351" src="https://github.com/user-attachments/assets/d46371de-c6fe-4348-8415-d39dfb8f2ef2" />

    Payload yang digunakkan:

    ```bash
    '+UNION+SELECT+@@version,NULL--+
    ```
    
    <img width="1917" height="969" alt="Cuplikan layar 2025-09-10 223049" src="https://github.com/user-attachments/assets/e194f0be-a318-44a6-beb9-d58f809fde07" />

    Pada bagian bawah tertera Versi nya.
---


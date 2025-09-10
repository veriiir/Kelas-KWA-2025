## Write-up: NoSQL Manipulation

### Overview

Title: Manipulasi Revies produk secara keseluruhan.

Category: SQL Injection

Describe: Update multiple product reviews at the same time.

<img width="439" height="227" alt="Cuplikan layar 2025-09-10 201355" src="https://github.com/user-attachments/assets/1eabb82c-2500-4fa8-8a6f-9d766b1390bb" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

## Resource

https://portswigger.net/web-security/nosql-injection

### Metodologi dan Solusi

### Langkah-Langkah

1. **Buka dan tulis review di salah satu product.**
   
   <img width="1919" height="965" alt="Cuplikan layar 2025-09-10 194028" src="https://github.com/user-attachments/assets/895d287a-dc62-4963-8c63-a72fc1927eb3" />

2. **Setelah menulis review, edit salah satu review yang dibuat.**
   
   <img width="1919" height="967" alt="Cuplikan layar 2025-09-10 194101" src="https://github.com/user-attachments/assets/0b4d9a04-23c4-4658-8a2f-25cf50c349fd" />

3. **Sebelum submit hasil edit review, aktifkan intercept di Burp Suite terlebih dahulu.**

   <img width="1919" height="1018" alt="Cuplikan layar 2025-09-10 195408" src="https://github.com/user-attachments/assets/f0199a6d-5d40-4dbd-8056-0cb7c3779975" />

   Ada sebuah request edit review dengan metode **PUT.**
   
5. **Pada bagian kanan terdapat request attributes, lalu ubah PUT menjadi PATCH.**

  <img width="1919" height="1013" alt="Cuplikan layar 2025-09-10 195521" src="https://github.com/user-attachments/assets/5c67cc7c-c313-4979-8428-6879de3617a7" />

6. **Setelah itu, Edit bagian Message dan Author menjadi id dan Message**
   
   Ubah isi dengan parameter id menjadi {"$ne": null}.
   
   Bagian request sebelum diedit:
   
   <img width="1919" height="1015" alt="Cuplikan layar 2025-09-10 195546" src="https://github.com/user-attachments/assets/be33373e-b52e-49e0-9612-c90c94d1fb17" />

   Bagian reguest setelah diedit:
   
   <img width="1919" height="1017" alt="Cuplikan layar 2025-09-10 195610" src="https://github.com/user-attachments/assets/c175a3f3-114f-45ec-8a7b-aab40d1d19d2" />
  
   Hasil:
   
   <img width="1919" height="962" alt="Cuplikan layar 2025-09-10 195237" src="https://github.com/user-attachments/assets/e4ddd529-4a55-4740-ba60-c8870a44ec9c" />

   Semua review langsung berubah.

### Catatan

Adanya payload {"$ne": null} pada payload NoSQL secara efektif mengabaikan fungsi yang seharusnya memperbarui review tertentu saja, sehingga mengakibatkan pembaruan massal untuk semua review dalam database.

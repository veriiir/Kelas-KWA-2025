## Write-up: Database Schema

## Overview

Title: SQL Injection on Product Filter

Category: SQL Injection

Describe: Exfiltrate the entire DB schema definition via SQL Injection.

<img width="426" height="226" alt="Cuplikan layar 2025-09-10 153130" src="https://github.com/user-attachments/assets/767d919c-7a64-4203-8b55-b44e3fb489bb" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

### Metodologi dan Solusi

### Langkah-Langkah

1. **Identifikasi Endpoint**

    Setelah mencegat traffic aplikasi menggunakan Burp Suite, ditemukan request dengan parameter q yang digunakan untuk memfilter produk. Parameter ini terlihat rawan disuntikkan query SQL.
    <img width="1919" height="1019" alt="Cuplikan layar 2025-09-10 145643" src="https://github.com/user-attachments/assets/7e417c61-1932-4630-a023-55553da6a730" />

2. **Kirim ke Repeater**

    Request tersebut dipindahkan ke tab Repeater di Burp Suite agar bisa diuji coba berulang kali dengan payload berbeda.
     <img width="1919" height="1019" alt="Cuplikan layar 2025-09-10 145917" src="https://github.com/user-attachments/assets/fb304067-e0fc-4ee7-ad39-cf59ebee0cf2" />

3. **Uji Payload Awal**

    Dicoba menyuntikkan payload sederhana untuk memastikan apakah parameter rentan terhadap SQL Injection.
   <img width="1919" height="1013" alt="Cuplikan layar 2025-09-10 145655" src="https://github.com/user-attachments/assets/ccfa15fd-6bed-4120-9ad6-36122d8ba5d3" />
   
4. **Menentukan Jumlah Kolom untuk UNION**

    Karena serangan akan menggunakan UNION, maka jumlah kolom harus sesuai. Dilakukan percobaan dengan menambahkan 'a' pada payload hingga tidak muncul error lagi.

    - Payload awal:
    ```bash
    apple'))+UNION+SELECT+'4'--
    ```
    <img width="1919" height="1014" alt="Cuplikan layar 2025-09-10 151348" src="https://github.com/user-attachments/assets/f9cf1e3c-e707-4117-8132-dad8e13438a7" />

    Setelah percobaan bertahap, payload berikut tidak error:
    ```bash
    apple'))+UNION+SELECT+'4','4','4','4','4','4','4','4','4'--
    ```
    <img width="1919" height="1015" alt="Cuplikan layar 2025-09-10 151325" src="https://github.com/user-attachments/assets/8ffb0ced-c97d-49c3-be7a-f6b1ef5a81c6" />

    Hasil ini menunjukkan bahwa query asli memiliki 9 kolom.

5. **Ekstraksi Schema Database**

    Setelah mengetahui jumlah kolom, dilakukan percobaan untuk menampilkan struktur database dari tabel internal SQLite:
    ```bash
    apple'))+UNION+SELECT+'4','4','4','4','4','4','4','4',sql+FROM+sqlite_schema--
    ```
    <img width="1919" height="1016" alt="Cuplikan layar 2025-09-10 151255" src="https://github.com/user-attachments/assets/8af7c955-5f2b-4a2f-bd19-da674260aa2b" />

    Dari hasil response, terlihat bahwa database yang digunakan adalah SQLite, dan struktur tabel berhasil terekspos.

### Hasil

- Berhasil menemukan jumlah kolom (9 kolom).

- Berhasil mengekstrak schema database menggunakan sqlite_schema.

- Teridentifikasi bahwa database yang digunakan oleh aplikasi adalah SQLite.

### Penjelasan Solusi

Serangan ini berhasil karena aplikasi tidak memvalidasi input pada parameter q dan tidak menggunakan prepared statement untuk query SQL. Hal ini memungkinkan penyerang untuk menyuntikkan kode tambahan (UNION SELECT ...) yang mengubah query asli, sehingga data internal (seperti schema database) dapat terekspos.

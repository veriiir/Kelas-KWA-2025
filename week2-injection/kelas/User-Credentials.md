## Write-up: User-Credentials

### Overview

Title: Retrieve a list of all user credentials via SQL Injection.

Category: SQL Injection

Describe: Retrieve a list of all user credentials via SQL Injection.

<img width="420" height="223" alt="Cuplikan layar 2025-09-10 203953" src="https://github.com/user-attachments/assets/86b8de8d-ea91-41c8-a2ed-849413930863" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

### Resource

https://portswigger.net/web-security/sql-injection/examining-the-database

https://portswigger.net/web-security/sql-injection/union-attacks

### Langkah - langkah

1. **Mendapatkan Schema Tabel Users (Dari soal Database Schema)**
   
<img width="1919" height="1018" alt="Cuplikan layar 2025-09-10 203140" src="https://github.com/user-attachments/assets/702314d2-4e38-46ed-ba09-9f2dee9704be" />

Pada soal sebelumnya kita sudah berhasil membaca struktur database. Dari tabel Users terdapat 13 kolom:
```bash
id, username, email, password, role, deluxeToken, lastLoginIp, profileImage, totpSecret, isActive, createdAt, updatedAt, deletedAt
```
Karena kita sudah tahu nama tabel dan kolomnya, kita bisa langsung membuat payload tanpa perlu menebak tipe dan jumlah kolom terlebih dahulu.

2. **Intercept Request Endpoint Rentan**

Endpoint yang digunakan:
```bash
/rest/products/search?q=
```

<img width="1919" height="1014" alt="Cuplikan layar 2025-09-10 201854" src="https://github.com/user-attachments/assets/5f1e5ffb-45ee-4ab3-b4f3-1b220760b986" />

Gunakan Burp Suite untuk mencegat permintaan GET ke endpoint tersebut.

Pindahkan request ke tab Repeater agar bisa diuji dengan payload yang berbeda-beda.

3. **Menyusun Payload UNION SELECT**

Berdasarkan pengujian sebelumnya, query filter produk pada endpoint ini memiliki 9 kolom.

Maka payload yang disuntikkan harus menyesuaikan dengan jumlah kolom tersebut.

Contoh payload untuk membaca data sensitif dari tabel Users:

```bash
aaa'))+UNION+SELECT+username,email,password,role,deluxeToken,totpSecret,isActive,createdAt,updatedAt+FROM+Users--
```

Payload ini memilih 9 kolom pertama yang relevan dari tabel Users dan menyamakan jumlah kolom dengan query asli agar tidak terjadi error.

4. **Mengirimkan Payload**

Kirimkan request yang telah dimodifikasi ke server melalui tab Repeater.

Response server akan menampilkan data sensitif dari tabel Users seperti username, email, password, role, hingga status isActive.

<img width="1918" height="1019" alt="Cuplikan layar 2025-09-10 203517" src="https://github.com/user-attachments/assets/552f4bcf-d0f0-43e6-89e6-de5467baab49" />

### Catatan

Karena struktur tabel sudah diketahui sebelumnya, payload dapat langsung disesuaikan tanpa harus mencoba-coba tipe dan jumlah kolom.

Kolom totpSecret yang tidak kosong dapat memicu sistem menganggap akun memiliki 2FA, walaupun tokennya sebenarnya tidak valid.

Selalu pastikan jumlah kolom pada UNION SELECT sama dengan query asli, jika tidak server akan mengembalikan error.

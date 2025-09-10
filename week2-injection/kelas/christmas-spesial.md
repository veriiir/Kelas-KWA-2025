## Write-up: Christmas Spesial

### Overview

Title: Tambahkan Produk Tersembunyi ke Keranjang

Category: SQL Injection & Business Logic

Describe: Memanfaatkan data schema yang telah terekspos untuk menambahkan produk yang sudah dihapus (“Christmas Special”) ke keranjang belanja.

<img width="444" height="227" alt="Cuplikan layar 2025-09-10 162843" src="https://github.com/user-attachments/assets/ce939616-2b62-4d0a-8635-3f99bfb6bc93" />

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- ***Burp Suite:** Digunakan untuk mencegat dan mengubah permintaan HTTP guna menyuntikkan (inject) kode SQL.

### Metodologi dan Solusi

### Langkah-Langkah

1. **Menemukan Produk “Christmas Special”**

    Karena endpoint yang digunakan sama seperti saat mengakses schema database, kita dapat langsung membuat payload berdasarkan struktur tabel yang telah ditemukan sebelumnya.

    Payload:
    ```bash
    aaaa')) UNION SELECT * FROM Products--
    ```
    <img width="1919" height="1017" alt="Cuplikan layar 2025-09-10 155946" src="https://github.com/user-attachments/assets/393fd429-3ed1-4dbb-83ce-1e5a0afabd46" />

    Hasil:
    Dari response server terlihat bahwa ProductId untuk “Christmas Special” adalah 10.

2. **Menambahkan Produk ke Keranjang**

    Tambahkan produk apa saja ke keranjang seperti biasa melalui aplikasi.

    Intercept permintaan (request) penambahan ke keranjang menggunakan Burp Suite.
    
    <img width="1919" height="1015" alt="Cuplikan layar 2025-09-10 161838" src="https://github.com/user-attachments/assets/bd7f1bbb-858a-4fe4-a1f0-a2e97ef76193" />

    Ubah parameter ProductId di body/parameter request menjadi 10 (ID produk “Christmas Special”).
   
   <img width="1919" height="1018" alt="Cuplikan layar 2025-09-10 162016" src="https://github.com/user-attachments/assets/33d0fbf8-0efe-477c-95e1-bf7af14057cf" />

    Contoh sebelum modifikasi:

    {"ProductId":1,"Quantity":1}

    Contoh setelah modifikasi:

    {"ProductId":10,"Quantity":1}

2. **Verifikasi di Keranjang**

    Setelah mengirimkan request yang dimodifikasi, buka halaman keranjang belanja. Produk “Christmas Special” sekarang muncul dan berhasil ditambahkan.
   
    <img width="1919" height="1021" alt="Cuplikan layar 2025-09-10 162517" src="https://github.com/user-attachments/assets/ed4f8958-be78-4d7a-8b34-5f83763eb7f0" />

### Catatan

Karena struktur tabel Products sudah diperoleh pada langkah sebelumnya, kita bisa langsung menggunakan UNION SELECT * FROM Products untuk membaca semua data produk.

Query SQL aplikasi tampaknya hanya menampilkan produk yang belum dihapus (deletedAt IS NULL). Dengan payload ini, produk yang sudah dihapus (seperti “Christmas Special 2014”) tetap muncul dan bisa dipesan.

### Penjelasan Solusi

Kelemahan ini terjadi karena:

Kurangnya validasi input pada parameter filter (q).

Tidak ada pembatasan akses saat melakukan permintaan menambahkan produk ke keranjang (server tidak mengecek apakah produk valid / aktif).

Akibatnya, penyerang dapat memanfaatkan informasi schema database untuk mengakses dan memesan produk yang seharusnya sudah tidak tersedia.

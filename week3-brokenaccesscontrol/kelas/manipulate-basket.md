# Write-up: Manipulate Basket

## Overview

Judul: Manipulate Basket

Kategori: Broken Access Control

Penjelasan: Tantangan “Manipulate Basket” melibatkan penambahan item ke keranjang belanja pengguna lain dengan mengeksploitasi potensi kerentanan Insecure Direct Object References (IDOR) dalam aplikasi.

<img width="436" height="202" alt="Cuplikan layar 2025-09-16 230329" src="https://github.com/user-attachments/assets/6de32442-b086-4477-a11d-9b3823e9146a" />

## Alat yang Digunakan

**Browser Web**: Untuk berinteraksi dengan aplikasi web dan memeriksa permintaan jaringan.

**Burp Suite**: Untuk mencegat, memodifikasi, dan mengirim ulang permintaan HTTP.

## Metodologi dan Solusi

1. **Menjelajahi Fungsionalitas Keranjang**

Menambahkan produk ke keranjang pribadi untuk mengamati struktur permintaan HTTP dan parameter yang terlibat.
Diketahui setiap operasi keranjang (tambah, ubah) direferensikan oleh ID keranjang tertentu yang terkait dengan pengguna.

<img width="1177" height="725" alt="Cuplikan layar 2025-09-16 231026" src="https://github.com/user-attachments/assets/59e10f7e-bdf5-40d0-8a26-24ea96cf1639" />

2. **Manipulasi Permintaan**

Mencegat permintaan saat produk ditambahkan ke keranjang menggunakan Burp Suite.
Memperhatikan bahwa BasketId tercantum di dalam body permintaan, yang menunjukkan potensi vektor IDOR.

<img width="1490" height="748" alt="Cuplikan layar 2025-09-16 230456" src="https://github.com/user-attachments/assets/61a4aac0-1468-4b98-b6bb-fc0e08f4c230" />

3. **Memanipulasi BasketId**

Awalnya mencoba mengganti BasketId dalam permintaan yang dicegat ke ID keranjang pengguna lain untuk menguji adanya masalah direct object reference.
Namun, muncul pemeriksaan kontrol akses yang mencegah penambahan item ke keranjang yang bukan milik pengguna.

<img width="1489" height="735" alt="Cuplikan layar 2025-09-16 230541" src="https://github.com/user-attachments/assets/42a7ed9c-086d-433a-a6d1-d58af8ef3079" />

4. **Melewati Pemeriksaan Keamanan**

Mencoba melewati mekanisme keamanan dengan menduplikasi parameter BasketId dalam body permintaan (overload).
Awalnya muncul error (foreign key error), tetapi hal itu menunjukkan adanya reaksi server.
Setelah beberapa percobaan, ditemukan bahwa server hanya memproses parameter BasketId pertama untuk pemeriksaan keamanan, tetapi menggunakan BasketId kedua untuk operasi sebenarnya, sehingga berhasil menambahkan item ke keranjang pengguna lain.

<img width="1484" height="740" alt="Cuplikan layar 2025-09-16 230643" src="https://github.com/user-attachments/assets/2a410531-92c8-4814-abb4-2c97b8d7fef7" />

5. **Memastikan Eksploitasi**

Mengirim permintaan yang telah dimanipulasi dan menerima respons sukses.
Memeriksa keranjang pengguna target untuk memastikan item berhasil ditambahkan.

<img width="1468" height="740" alt="Cuplikan layar 2025-09-16 230721" src="https://github.com/user-attachments/assets/5c06522f-8d68-4dfe-a278-6771874ba35e" />

## Penjelasan Solusi

Tantangan ini berhasil diselesaikan dengan mengidentifikasi dan mengeksploitasi kelemahan dalam penanganan parameter permintaan HTTP, di mana aplikasi gagal mengamankan operasi keranjang terhadap modifikasi yang tidak sah. Dengan menyisipkan parameter BasketId ganda, pemeriksaan keamanan server dapat dilewati sehingga isi keranjang pengguna lain dapat dimanipulasi.

# Write-up: Admin Section

## Overview

Judul: Admin Section

Kategori: Broken Access Control

Tantangan “Admin Section” melibatkan upaya mengakses area administratif yang dibatasi pada sebuah aplikasi web.

<img width="430" height="219" alt="Cuplikan layar 2025-09-16 094427" src="https://github.com/user-attachments/assets/a9e79b1e-0a63-4c26-a507-fcec090bafa7" />

## Alat yang Digunakan

**Browser Web**: Untuk mengakses aplikasi dan memanfaatkan developer tools guna memeriksa file sumber.

**Developer Tools**: Khususnya penampil kode sumber untuk memeriksa file JavaScript dan definisi rute (route).

## Metodologi dan Solusi

1. **Analisis Kode Sumber:**

Menggunakan developer tools browser, file main.js diperiksa. File ini berisi definisi berbagai rute aplikasi (path dan komponen), termasuk path untuk bagian “administration”.
Pengaturan rute ini menunjukkan bahwa area administrasi bisa diakses langsung jika path yang benar diketahui.

<img width="1919" height="977" alt="Cuplikan layar 2025-09-16 094521" src="https://github.com/user-attachments/assets/fbda705c-e2dd-411f-aac5-3f50071608d1" />

2. **Akses Path Langsung**

Dengan memasukkan alamat berikut di bilah alamat browser:

```bash
https://juice-shop.herokuapp.com/#/administration
```

Awalnya, pendekatan ini menghasilkan halaman access denied karena izin tidak mencukupi.

<img width="1919" height="967" alt="Cuplikan layar 2025-09-16 094550" src="https://github.com/user-attachments/assets/dc80a3af-0f53-44a2-9e7a-214c2a6ddaca" />

3. **Mendapatkan Akses Admin**

Login menggunakan kredensial akun administrator.
Kemudian kembali membuka path /administration yang kini berhasil menampilkan dashboard administrasi, menunjukkan adanya kontrol akses yang tidak tepat dan hanya mengandalkan kerahasiaan path.

<img width="1919" height="972" alt="Cuplikan layar 2025-09-16 094629" src="https://github.com/user-attachments/assets/c1a8c48b-2487-4a80-81a5-3b4504527fcd" />

## Penjelasan Solusi

Tantangan ini diselesaikan dengan menemukan path administratif tersembunyi dari file JavaScript sumber lalu mengaksesnya menggunakan kredensial administrator. Hal ini menunjukkan kerentanan aplikasi di mana area sensitif tidak diamankan dengan benar, hanya “disembunyikan” dengan asumsi pengguna tidak akan menemukan path tersebut.

# Write-up: View Basket

## Overview

Judul: View Basket

Kategori: Broken Access Control

Penjelasan: Tantangan “View Basket” berfokus pada eksploitasi kerentanan broken access control dalam sebuah aplikasi web. Kerentanan spesifiknya memungkinkan pengguna melihat detail keranjang belanja milik pengguna lain dengan memanipulasi pengenal (identifier) spesifik pengguna pada permintaan web.

<img width="429" height="227" alt="Cuplikan layar 2025-09-16 095425" src="https://github.com/user-attachments/assets/3bdaf55c-0330-4fdd-bea6-d392f90894a1" />

## Alat yang Digunakan

**Browser Web**: Untuk mengakses aplikasi dan memanfaatkan developer tools guna memeriksa file sumber.

**Developer Tools**: Khususnya penampil kode sumber untuk memeriksa file JavaScript dan definisi rute (route).

## Metodologi dan Solusi

### Langkah Penyelesaian

1. **Melakukan Login Pengguna**

Untuk mengetahui isi keranjang sendiri terlebih dahulu, lakukan login pengguna bebas, bisa admin dan sebagainya. Disini saya login sebagai admin

<img width="1919" height="927" alt="Cuplikan layar 2025-09-16 095604" src="https://github.com/user-attachments/assets/3bd213ed-3a61-4f52-aeb8-d0a14bba404e" />

Angka 1 menunjukkan ID keranjang untuk pengguna saat ini.

<img width="1157" height="773" alt="Cuplikan layar 2025-09-16 100453" src="https://github.com/user-attachments/assets/9c1c2ac7-15f9-4f68-a14c-ccfc3dcd703b" />

2. **Memodifikasi ID Keranjang**

Mengubah ID keranjang dalam permintaan yang dicegat dari 1 menjadi 2 untuk mencoba mengakses detail keranjang pengguna lain. Untuk memodifikasi bisa dilakukan di *burpsuit*, namun bisa juga dilakukan pada *developer tools* dengan masuk di application kemudian cari session storage.

id keranjang awal:

<img width="1919" height="970" alt="Cuplikan layar 2025-09-16 095814" src="https://github.com/user-attachments/assets/8e9e4025-eaed-42ce-a0b3-ba5b62ca70aa" />

id keranjang setelah dimodifikasi:

<img width="1919" height="967" alt="Cuplikan layar 2025-09-16 095838" src="https://github.com/user-attachments/assets/d6c60249-c5e4-4430-bf6b-b5bde2239dbb" />

3. **Melihat Hasil**

Permintaan yang telah dimodifikasi diteruskan, dan respons berisi detail keranjang milik pengguna lain, mengonfirmasi adanya kerentanan broken access control.

<img width="1173" height="523" alt="Cuplikan layar 2025-09-16 100512" src="https://github.com/user-attachments/assets/f9dcd9f9-82aa-4471-b4b2-665f2c19df48" />

## Penjelasan Solusi

Tantangan ini berhasil diselesaikan dengan mengubah ID keranjang pada URL permintaan. Hal ini seharusnya tidak mungkin dilakukan apabila mekanisme kontrol akses diterapkan dengan benar. Dengan kelemahan tersebut, akses ke informasi keranjang pengguna lain menjadi mungkin.

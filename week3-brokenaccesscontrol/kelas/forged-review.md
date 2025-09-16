# Write-up: Forged Review

## Overview

Judul: Forged Review

Kategori: Broken Access Control

Penjelasan: Tantangan “Forged Review” melibatkan manipulasi proses pengiriman ulasan pada sebuah aplikasi web untuk memposting ulasan atas nama pengguna lain, dengan memanfaatkan lemahnya mekanisme kontrol akses.

<img width="427" height="211" alt="Cuplikan layar 2025-09-16 221233" src="https://github.com/user-attachments/assets/c42c106c-2340-477f-9ae9-254ff8a0575a" />

## Alat yang Digunakan

**Burp Suite**: Untuk mencegat dan memanipulasi permintaan HTTP.

## Metodologi dan Solusi

1. **Mengidentifikasi Kerentanan**

Mengirim Ulasan Secara Normal:

Memulai dengan mengirim ulasan generik melalui antarmuka aplikasi web.

<img width="1919" height="973" alt="Cuplikan layar 2025-09-16 221955" src="https://github.com/user-attachments/assets/169c0335-8950-43b2-82e9-64114ea43e9f" />

Mencegat Permintaan:

Menggunakan Burp Suite untuk mencegat permintaan HTTP yang dikirim saat mengirim ulasan.

<img width="1919" height="1019" alt="Cuplikan layar 2025-09-16 222519" src="https://github.com/user-attachments/assets/99db5766-43a8-42f8-bd90-9c4351b94254" />

Menganalisis Parameter Permintaan:

Ditemukan bahwa payload permintaan mencakup parameter author yang secara manual menentukan penulis ulasan. Ini mengindikasikan server tidak memvalidasi apakah pengguna yang mengirim ulasan benar-benar sesuai dengan penulis yang tercantum pada permintaan.

2. **Mengeksploitasi Kerentanan**

Mengubah Parameter Author:

Mengganti nilai parameter author dalam permintaan HTTP menjadi nama atau pengenal pengguna lain.

<img width="1919" height="1019" alt="Cuplikan layar 2025-09-16 222528" src="https://github.com/user-attachments/assets/7dc5d1b0-b25b-410f-924b-3a74a9bb79b6" />

Mengirim Ulang Permintaan:

Mengirim ulang permintaan yang sudah dimodifikasi ke server menggunakan Burp Suite.

Memverifikasi Hasil:

Memeriksa aplikasi untuk memastikan ulasan muncul dengan nama author yang telah diubah, mengonfirmasi adanya kerentanan.

<img width="1919" height="975" alt="Cuplikan layar 2025-09-16 222650" src="https://github.com/user-attachments/assets/264d169a-e254-4aeb-b1b0-12a91a66ea2a" />

3. **Manipulasi Tambahan**

Mengedit Ulasan yang Ada:

Menemukan bahwa ulasan yang sudah ada juga dapat diedit dengan mencegat dan memanipulasi permintaan edit dengan cara serupa.

<img width="740" height="735" alt="Cuplikan layar 2025-09-16 224204" src="https://github.com/user-attachments/assets/071998de-c84e-466d-9acf-19f462cc78d4" />

Mengidentifikasi ID Ulasan:

Mendapatkan ID ulasan dari fitur lain seperti fitur “like”, di mana permintaan yang dicegat mengungkapkan ID ulasan.

<img width="740" height="735" alt="Cuplikan layar 2025-09-16 224204" src="https://github.com/user-attachments/assets/3db844fb-891d-40d9-aa05-f718924f30b0" />

Memanipulasi Ulasan Berdasarkan ID:

Menggunakan ID yang diperoleh untuk mengedit ulasan milik pengguna lain dengan menyuntikkan ID tersebut ke dalam permintaan edit sehingga konten ulasan yang bukan milik penyerang dapat diubah.

<img width="1487" height="742" alt="Cuplikan layar 2025-09-16 224820" src="https://github.com/user-attachments/assets/bb7a2558-1c00-4909-9539-6ad4406d0fb4" />

Hasil:

<img width="643" height="601" alt="Cuplikan layar 2025-09-16 224836" src="https://github.com/user-attachments/assets/423a6d5d-84b6-4a9e-b5d5-109895fa9195" />

## Penjelasan Solusi

Tantangan ini menyoroti kelemahan keamanan signifikan terkait validasi input dan manajemen sesi:

- Kurangnya Validasi di Sisi Server: Server gagal memvalidasi apakah pengguna yang sedang login sesuai dengan author ulasan.

- Insecure Direct Object References (IDOR): Aplikasi tidak mengamankan ID ulasan dengan baik sehingga penyerang dapat memodifikasi ulasan apa pun hanya dengan mengetahui ID-nya.

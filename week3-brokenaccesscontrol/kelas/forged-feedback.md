# Write-up: Forged Feedback

## Overview

Judul: Forged Feedback

Kategori: Broken Access Control

Penjelasan: Tantangan “Forged Feedback” melibatkan eksploitasi kontrol akses yang lemah untuk memposting umpan balik (feedback) atas nama pengguna lain, yang menunjukkan kerentanan terkait manajemen identitas pengguna pada sebuah aplikasi web.

<img width="439" height="205" alt="Cuplikan layar 2025-09-16 114718" src="https://github.com/user-attachments/assets/573c4904-85ee-4409-8eff-beeee17ce9f8" />

## Alat yang Digunakan

**Burp Suite**: Untuk mencegat dan memanipulasi permintaan HTTP.

## Metodologi dan Solusi

1. **Mengirim Feedback Secara Normal**

Awalnya mengirim feedback melalui antarmuka aplikasi untuk memahami bagaimana proses pengiriman feedback berlangsung.

<img width="682" height="724" alt="Cuplikan layar 2025-09-16 103257" src="https://github.com/user-attachments/assets/4de3e60d-18e5-4d24-ae98-9f8419b52d2f" />

2. **Mencegat Permintaan**

Menggunakan Burp Suite untuk menangkap permintaan HTTP saat feedback dikirim. Analisis permintaan untuk memahami strukturnya dan parameter yang digunakan.

<img width="1919" height="1013" alt="Cuplikan layar 2025-09-16 105618" src="https://github.com/user-attachments/assets/91088d39-b0be-4dba-9d40-85c000805d5c" />

3. **Mengidentifikasi Mekanisme Identifikasi Pengguna**

Permintaan pengiriman feedback mencakup pengenal pengguna, misalnya parameter user_id atau serupa, yang digunakan aplikasi untuk mengaitkan feedback dengan pengguna tertentu.

<img width="1919" height="1014" alt="Cuplikan layar 2025-09-16 105020" src="https://github.com/user-attachments/assets/896ae35f-a215-49fc-ae82-82bd895632b2" />

4. **Memanipulasi Pengenal Pengguna**

Mengubah nilai parameter user_id dalam permintaan yang dicegat ke ID pengguna lain, misalnya pengguna dengan hak lebih tinggi seperti admin, untuk menguji apakah aplikasi menerapkan pengecekan otorisasi dengan benar.
Permintaan yang telah dimodifikasi kemudian dikirim ulang untuk melihat apakah feedback diposting atas nama pengguna lain.

<img width="1919" height="1017" alt="Cuplikan layar 2025-09-16 105039" src="https://github.com/user-attachments/assets/d8dc888a-6612-4c56-9d30-1b6359c50ad7" />

5. **Memverifikasi Hasil**

Memeriksa aplikasi untuk memastikan apakah feedback muncul di profil atau riwayat feedback pengguna lain.

<img width="756" height="475" alt="Cuplikan layar 2025-09-16 105415" src="https://github.com/user-attachments/assets/27d0155f-e6aa-4fe0-ba75-f26ec52ac320" />

## Penjelasan Solusi

Tantangan ini berhasil diselesaikan dengan memanipulasi pengenal pengguna dalam proses pengiriman feedback. Hal ini dimungkinkan karena server gagal memvalidasi apakah pengguna yang mengirim feedback sama dengan user_id yang tercantum dalam permintaan. Jenis kerentanan ini merupakan contoh broken access control di mana aplikasi tidak memverifikasi identitas atau izin pengguna secara memadai sebelum melakukan tindakan atas nama mereka.

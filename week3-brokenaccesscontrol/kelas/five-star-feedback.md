# Write-up: Five-Star Feedback

# Overview

Judul: Five-Star Feedback

Kategori: Broken Access Control

Penjelasan: Tantangan “Five-Star Feedback” mengharuskan manipulasi sistem ulasan pada sebuah aplikasi web dengan cara menghapus semua ulasan bintang lima.

<img width="428" height="231" alt="Cuplikan layar 2025-09-16 101917" src="https://github.com/user-attachments/assets/f0b89b04-5296-42f1-90fa-84fd44447f15" />

## Alat yang Digunakan

**Browser Web**: Untuk berinteraksi dengan dashboard administrasi aplikasi.

**Akses ke Panel Admin**: Memanfaatkan akses administratif yang diperoleh sebelumnya untuk menavigasi dan memodifikasi data aplikasi.

## Metodologi dan Solusi

1. **Mengakses Panel Admin** (Admin-section)

Memanfaatkan Hak Istimewa Admin:

Berdasarkan tantangan sebelumnya, akses administrator sudah diperoleh sehingga memungkinkan untuk menjelajahi fitur yang tidak tersedia bagi pengguna biasa, termasuk pengelolaan umpan balik (feedback) pengguna.

2. **Menghapus Ulasan Bintang Lima**

Identifikasi Ulasan Bintang Lima:

<img width="745" height="743" alt="Cuplikan layar 2025-09-16 101326" src="https://github.com/user-attachments/assets/a02c34a5-d2ab-405f-b30b-83f3b38b2865" />

Ulasan ditampilkan dengan rating bintang masing-masing. Semua entri dengan rating lima bintang diidentifikasi untuk dihapus.

<img width="741" height="731" alt="Cuplikan layar 2025-09-16 101517" src="https://github.com/user-attachments/assets/3d9a87ff-e615-4829-86cf-25b626d8f95f" />

## Penjelasan Solusi

Tantangan ini diselesaikan dengan mengeksploitasi lemahnya kontrol akses yang seharusnya membatasi penghapusan umpan balik hanya kepada pihak yang berwenang. Bahkan untuk pihak berwenang sekalipun, sistem seharusnya tidak mengizinkan penghapusan massal tanpa pengawasan atau alasan yang jelas.

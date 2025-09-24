## Alat yang Digunakan
# Write-up: Imaginary Challenge

## Overview

Judul: Imaginary Challenge

Kategori: Cryptographic Issues

“Imaginary Challenge” adalah tantangan metaforis di OWASP Juice Shop yang sebenarnya tidak sepenuhnya ada secara fungsional. Tujuannya untuk menekankan pemahaman lingkungan aplikasi dan berpikir kreatif di luar alur normal CTF.

*Catatan Penting: Challenge ini memang kurang terdokumentasi. Pengguna dianjurkan untuk membuka pull request jika menemukan cara penyelesaian baru.*

## Alat yang Digunakan

**Web Browser** – untuk berinteraksi dengan aplikasi Juice Shop.

**Code Review** – khususnya memeriksa package.json dan skrip terkait untuk mencari petunjuk.

## Metodologi dan Solusi

1. **Analisis Awal**

Karena petunjuk challenge mengisyaratkan bahwa tantangan ini “tidak ada”, langkah pertama adalah meneliti mekanisme internal Juice Shop, khususnya bagaimana ContinueCodes digunakan untuk memvalidasi penyelesaian challenge.

2. **Menyelidiki package.json**

Dalam package.json ditemukan dependensi bernama hashid yang digunakan untuk encoding/decoding ID seperti hash.

Ini mengarah pada asumsi bahwa hashid juga dapat dipakai untuk menghasilkan atau memecahkan ContinueCode.

3. **Riset Tambahan & Konsultasi Write-up Komunitas**

Mencari write-up komunitas dan catatan lama.

Ternyata, pada masa lalu hashid memiliki halaman demo (CodePen) untuk menguji encoding.

Saat ini halaman itu sudah hilang setelah hashid diakuisisi perusahaan lain dan situsnya didesain ulang.

4. **Solusi Teoretis**

Jika fitur lama tersebut masih ada, solusi yang diperkirakan adalah:

Menggunakan demo hashid untuk mengenkripsi angka 999 dengan salt default bawaan demo.

Hasil encoding tersebut menghasilkan sebuah ContinueCode.

Apply kode itu melalui permintaan HTTP:
```bash

PUT http://localhost:3000/rest/continue-code/apply/{ContinueCode}
```

(ganti {ContinueCode} dengan output hashid).

Langkah itu akan memicu sistem Juice Shop untuk menandai challenge sebagai selesai.

## Penjelasan Solusi

Tantangan ini menunjukkan risiko menggunakan nilai default pada perangkat lunak. Jika default salt/secret diketahui publik (misalnya pada demo hashid), maka mekanisme validasi seperti ContinueCode dapat ditebak atau dipalsukan.

## Rekomendasi Perbaikan

Hindari Nilai Default: Selalu ubah salt, secret, atau key default saat menggunakan library pihak ketiga.

Audit Dependensi: Pastikan dependensi yang digunakan tidak mengekspos mekanisme keamanan internal ke klien.

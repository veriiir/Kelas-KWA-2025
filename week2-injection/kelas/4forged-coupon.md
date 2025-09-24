# Write-up: Forged Coupon

## Overview

Judul: Forged Coupon

Kategori: Cryptography

Tantangan ini meminta kita melakukan reverse-engineering untuk menemukan algoritme yang dipakai membuat kupon diskon, lalu memalsukan kupon valid agar bisa digunakan di aplikasi Juice Shop.

<img width="404" height="223" alt="Cuplikan layar 2025-09-24 194311" src="https://github.com/user-attachments/assets/044f740b-9b4e-44b6-b397-1c8243ee5aa7" />

## Alat yang Digunakan

**Web Browser** – untuk interaksi dengan Juice Shop.

**Cryptii** – untuk uji coba dan konfirmasi metode encoding.

## Metodologi dan Solusi

1. **Masuk ke direktori ftp**

Setelah masuk, lakukan tambahkan **%2500.md** pada file coupon dan package-lock untuk mendownload file tersebut.

<img width="1919" height="772" alt="Cuplikan layar 2025-09-24 190735" src="https://github.com/user-attachments/assets/218b21ea-a4a3-47b4-9a39-91cc3a8bdb38" />

<img width="1918" height="748" alt="Cuplikan layar 2025-09-24 190754" src="https://github.com/user-attachments/assets/5efc3f77-3929-4004-93fd-84cf9322cbc0" />

1. **Analisis Kupon yang Ada**

Buka file coupon dan package-lock untuk mengetahui isi filenya.

<img width="567" height="346" alt="Cuplikan layar 2025-09-24 191039" src="https://github.com/user-attachments/assets/2723219c-58a6-49ac-9940-e1f736b95e31" />

Lalu coba amati pola pada karakter dan panjang string untuk mencari keteraturan atau algoritme yang dipakai.

Contoh kupon:
```bash
n<MibgC7sn
mNYS#gC7sn
o*IVigC7sn
k#pDlgC7sn
o*I]pgC7sn
n(XRvgC7sn
n(XLtgC7sn
k#*AfgC7sn
q:<IqgC7sn
pEw8ogC7sn
pes[BgC7sn
l}6D$gC7ss
```

2. **Mengidentifikasi Library Encoding**

Menggunakan crypto detector online untuk menebak algoritme; tidak ditemukan yang umum → kemungkinan custom / jarang.

Memeriksa package.json (yang didapat dari FTP challenge) untuk mencari paket kriptografi.

Menemukan adanya library Z85 (ZeroMQ Base-85) yang sesuai untuk encoding string pendek.

<img width="611" height="494" alt="Cuplikan layar 2025-09-24 191057" src="https://github.com/user-attachments/assets/6df52662-4ed2-438a-a08e-0eda5581e326" />

3. **Mendekode Kupon**

Menggunakan Cryptii untuk mendekode kupon dengan algoritme Z85.

Hasil dekode menunjukkan format konsisten:

{3 huruf BULAN}{2 digit TAHUN}-{PERSENTASE DISKON}

<img width="1919" height="563" alt="Cuplikan layar 2025-09-24 191210" src="https://github.com/user-attachments/assets/30547cd6-31b6-4655-bf25-d2f7d87d2fbb" />

Contoh: SEP25-85 untuk diskon 85% September 2025.

4. **Membuat Kupon Palsu**

Membuat string plaintext sesuai format: SEP25-85.

Encode string ini dengan Z85 untuk menghasilkan kupon baru.

Input: SEP25-85

Output (encoded): q:<Irh7Z^B

<img width="1919" height="539" alt="Cuplikan layar 2025-09-24 194516" src="https://github.com/user-attachments/assets/ba4ae86e-d0fd-48c9-8345-469dee60efce" />

Kupon hasil encode terlihat sah karena pola sama dengan kupon asli.

5. **Menguji Kupon Palsu**

Memasukkan kupon hasil encode ke Juice Shop.

<img width="1133" height="858" alt="Cuplikan layar 2025-09-24 192122" src="https://github.com/user-attachments/assets/11f79f9c-1962-4e25-83d8-bb9ba96dc62f" />

Sistem menerima kupon dan menerapkan diskon → pemalsuan berhasil.

## Penjelasan Solusi

Kerentanan terjadi karena Juice Shop hanya memakai encoding yang dapat dibalik (Z85) untuk membuat kupon tanpa tanda tangan digital atau mekanisme validasi tambahan. Hal ini membuat kupon dapat diprediksi dan dipalsukan.

## Rekomendasi Perbaikan

Gunakan Randomisasi yang Aman: Buat kode kupon dengan metode kriptografi yang aman (misalnya token acak + tanda tangan server) agar tidak bisa ditebak atau dibalik.

Batasi Eksposur Algoritme: Jangan menaruh petunjuk algoritme di file klien seperti package.json atau sumber publik.

Tambahkan Validasi Server: Gunakan signature/HMAC pada kupon untuk memastikan hanya kupon yang dibuat server yang valid.

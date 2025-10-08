# Write-up: Error Handlin

## Ringkasan Tantangan

Judul: Error Handling

Kategori: Security Misconfiguration

Deskripsi: Tantangan ini meminta memicu sebuah error yang menampilkan detail backend akibat penanganan error yang tidak tepat atau verbose. Tujuannya untuk memahami bagaimana misconfigurations server dapat membocorkan informasi sensitif yang berguna bagi penyerang.

<img width="443" height="236" alt="Cuplikan layar 2025-10-08 224613" src="https://github.com/user-attachments/assets/81518153-b090-43cf-afbe-4b711e2e6dda" />

## Alat yang Digunakan

**Browser web** — untuk menavigasi dan memicu respon error.

**Pengetahuan konfigurasi server umum** — untuk menebak jalur/tipe file yang mungkin memicu error.

## Metodologi dan Solusi

1. **Eksperimen dengan Path URL**

Mengunjungi beberapa path yang tidak ada atau dibatasi pada server untuk memicu respons error.

<img width="1917" height="1022" alt="Cuplikan layar 2025-10-08 230541" src="https://github.com/user-attachments/assets/dec0844e-50bf-4812-b04d-f2c16a7b7877" />

2. **Analisis Pesan Error**

Memeriksa pesan error yang muncul secara teliti, mencari indikasi kebocoran konfigurasi atau detail backend.

<img width="1915" height="1020" alt="Cuplikan layar 2025-10-08 230607" src="https://github.com/user-attachments/assets/5159e6e5-c59e-4301-8d93-d252c7557390" />

3. **Memicu Error Spesifik**

Mencoba mengakses tipe file atau direktori yang kemungkinan tidak didukung oleh konfigurasi server (berdasarkan hint), untuk memprovokasi pesan error yang lebih informatif.

<img width="1919" height="973" alt="Cuplikan layar 2025-10-08 230948" src="https://github.com/user-attachments/assets/23644202-6e5e-477c-a89e-a2a19890b171" />

## Hasil & Penjelasan Solusi

Error diprovokasi dengan mencoba mengakses URL tidak valid yang mencoba memuat tipe file/direktori yang tidak didukung.

Server merespons dengan pesan error verbose yang mengungkapkan hal-hal berikut:

Status: 500 (kode status HTTP yang menandakan ada sesuatu yang salah pada server situs web, tetapi server tidak dapat secara spesifik mengidentifikasi masalahnya.).

Informasi sensitif: pesan error menampilkan bahwa aplikasi berjalan pada Express JS versi 4.21.0.

Informasi ini berguna bagi penyerang karena memperlihatkan teknologi backend dan versinya — memungkinkan pencarian eksploit atau kerentanan spesifik untuk versi Express tersebut.

## Rekomendasi Perbaikan singkat

Jangan tampilkan error detail ke pengguna: Tampilkan pesan umum pada produksi (mis. “Something went wrong”) dan log detail di server saja.

Konfigurasi logging yang aman: Pastikan stack traces dan detail konfigurasi hanya tersimpan di log internal yang terlindungi.

Patch & update: Pastikan framework (mis. Express) selalu diperbarui ke versi yang tidak rentan.

Pengetesan & hardening: Lakukan review konfigurasi server dan pengetesan error-handling sebagai bagian dari proses security hardening.

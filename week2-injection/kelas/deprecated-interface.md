# Write-up: Deprecated Interface

## Ringkasan Tantangan

Judul: Deprecated Interface

Kategori: Security Misconfiguration

Deskripsi: Tantangan ini memanfaatkan antarmuka lawas yang sudah tidak dipelihara dengan baik (deprecated) dan masih menyediakan fungsionalitas rentan—dalam kasus ini fitur unggah file pada halaman /complain.

<img width="441" height="229" alt="Cuplikan layar 2025-10-08 231114" src="https://github.com/user-attachments/assets/eb6a0ceb-e048-45d1-96db-d2e6f360860b" />

## Alat yang Digunakan

**Browser Web** — untuk berinteraksi dengan aplikasi dan mengunggah file.

**Developer Tools (DevTools)** — untuk memeriksa permintaan jaringan dan kode JavaScript (mis. main.js).

## Metodologi dan Solusi

1. **Mengidentifikasi Antarmuka Rentan**

Meninjau fitur aplikasi dan menemukan halaman /complain tampak usang setelah hadirnya chatbot baru, sehingga kemungkinan kurang diperhatikan/pemeliharaan.

<img width="1919" height="971" alt="Cuplikan layar 2025-10-08 231517" src="https://github.com/user-attachments/assets/447b0abb-175c-4a74-a348-234e00b1f8be" />

2. **Memeriksa Fitur Unggah File**

Menggunakan DevTools untuk membuka main.js dan menelaah logika uploader.

<img width="1919" height="968" alt="Cuplikan layar 2025-10-08 231946" src="https://github.com/user-attachments/assets/0ef6b064-fa43-4a9f-922c-cbea94cce369" />

Ditemukan upaya pembatasan tipe MIME (mis. application/pdf, text/xml, application/zip, dll.) di sisi klien.

3. **Menguji dan Memanipulasi Unggahan**

Mencoba mengunggah file XML biasa → ditolak oleh pemeriksaan klien.

Mencoba mengemasnya sebagai .xml.zip → awalnya gagal.

Memodifikasi nama file (menghilangkan .zip) setelah memilih file tetapi sebelum proses upload sehingga melewati pemeriksaan sisi klien → upload berhasil.

## Eksploitasi

Dengan berhasil mengunggah file XML yang sebenarnya — namun diterima karena manipulasi nama — penyerang dapat berpotensi melakukan serangan lanjutan (mis. XXE) atau menyajikan konten berbahaya.

## Penjelasan Solusi

Suksesnya eksfiltrasi/pengunggahan disebabkan lemahnya validasi sisi server dan ketergantungan berlebihan pada pemeriksaan sisi klien. Manipulasi nama file sebelum upload memperlihatkan bahwa validasi MIME/tipe hanya dilakukan di klien — bukan di server — sehingga tidak dapat diandalkan untuk keamanan.

## Rekomendasi Perbaikan

Validasi di Sisi Server Wajib: Selalu validasi tipe file, ukuran, dan konten di server (cek magic bytes / content-type sebenarnya), jangan hanya mengandalkan ekstensi atau pemeriksaan JavaScript.

Batasi Jenis File yang Diterima: Terapkan whitelist ketat dan lakukan sanitasi file (mis. scan antivirus, parsing aman jika XML).

Gunakan Penyimpanan Terisolasi: Simpan file user di lokasi yang tidak dapat dieksekusi dan dengan nama acak; jangan letakkan langsung di path yang dapat diakses/dijalankan.

Audit & Hapus Fitur Deprecated: Hentikan atau perbaiki antarmuka/fungsi yang tidak lagi dipelihara. Lakukan audit berkala untuk menemukan endpoint usang.

Logging & Monitoring: Catat upload yang mencurigakan dan berikan alert untuk pola yang tidak wajar.

<img width="754" height="746" alt="Cuplikan layar 2025-09-16 204223" src="https://github.com/user-attachments/assets/e734692c-1cd1-42ca-a14a-00ce1a9534dd" /># Write-up: CSRF

## Overview

Judul: CSRF (Cross-Site Request Forgery)

Kategori: Broken Access Control

Penjelasan: Tantangan “CSRF” melibatkan eksekusi serangan Cross-Site Request Forgery untuk mengubah nama pengguna pada platform OWASP Juice Shop tanpa persetujuan pemilik akun.

<img width="430" height="209" alt="Cuplikan layar 2025-09-16 120156" src="https://github.com/user-attachments/assets/79dd1ac4-5cdc-47c4-a8b2-d2acca29c84e" />

## Alat yang Digunakan

**Browser Web**: Secara khusus versi lama yang belum memiliki proteksi CSRF modern (contoh sederhana: Firefox versi 81.0).

**Burp Suite**: Untuk mencegat dan menganalisis permintaan HTTP.

**HTML Editor**: Untuk menyusun dan menjalankan halaman exploit CSRF (misalnya http://htmledit.squarefree.com/
).

## Metodologi dan Solusi

1. **Mengidentifikasi Endpoint yang Rentan**

Gunakan Burp Suite untuk mencegat dan memeriksa permintaan HTTP yang terjadi saat mengganti nama pengguna melalui halaman profil aplikasi.
Identifikasi permintaan POST yang digunakan untuk mengubah nama pengguna, khususnya URL dan parameter form yang dikirim.

<img width="754" height="746" alt="Cuplikan layar 2025-09-16 204223" src="https://github.com/user-attachments/assets/cfceb12d-7eba-4c51-8877-b40c273b96a4" />

2. Menyusun Halaman Serangan CSRF

Buat halaman HTML yang berisi form auto-submitting yang diarahkan ke endpoint yang rentan.
Gunakan field tersembunyi untuk menetapkan nilai username baru yang diinginkan.
Tambahkan JavaScript untuk mengirim form secara otomatis saat halaman dimuat.

Contoh:
```bash
<html>
  <body>
    <form action="http://localhost:3000/profile" method="POST">
      <input type="hidden" name="username" value="veriiir" />
    </form>
    <script>document.forms[0].submit();</script>
  </body>
</html>
```

3. **Menjalankan Serangan CSRF**

Unggah halaman HTML CSRF ke domain publik atau yang dikuasai. Untuk keperluan CTF ini, file diuji secara lokal menggunakan http://htmledit.squarefree.com/ untuk mensimulasikan skenario serangan nyata.
Ketika korban mengunjungi halaman berbahaya tersebut, skrip memicu pengiriman form menggunakan sesi autentikasi korban, sehingga nama pengguna berubah tanpa persetujuan eksplisit.

4. **Mengatasi Proteksi Browser Modern**

Karena browser modern telah meningkatkan proteksi terhadap serangan CSRF, gunakan versi browser yang lebih lama yang belum memiliki proteksi ini.
Matikan fitur keamanan yang biasanya memblokir atau memperingatkan permintaan lintas situs mencurigakan.

## Penjelasan Solusi

Tantangan ini berhasil diselesaikan dengan membuat dan mengirimkan halaman HTML yang secara otomatis mengirim permintaan POST ke endpoint yang rentan. Kelemahan kontrol akses memungkinkan perubahan data pengguna terjadi tanpa verifikasi atau persetujuan, menunjukkan adanya kerentanan Cross-Site Request Forgery.

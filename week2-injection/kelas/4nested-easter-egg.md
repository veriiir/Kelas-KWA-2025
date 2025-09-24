# Write-up: Nested Easter Egg

## Overview

Judul: Nested Easter Egg
Kategori: Cryptographic Issues

Tantangan “Nested Easter Egg” meminta kita untuk mendekripsi pesan tersembunyi yang berada di dalam Easter egg lain, menekankan analisis kriptografi tingkat lanjut untuk menemukan Easter egg yang sebenarnya dalam aplikasi.

<img width="427" height="191" alt="Cuplikan layar 2025-09-24 183750" src="https://github.com/user-attachments/assets/96c91a38-b209-43d2-963b-21801b1b5bfb" />

## Alat yang Digunakan

**Base64 Decoder**: Untuk mendekode string yang di-encode dengan Base64.

**ROT13 Cipher Tool**: Untuk mendekripsi pesan yang di-encode menggunakan substitusi cipher ROT13.

## Metodologi dan Solusi

1. **Masuk ke ftp**

Masuk ke direktori ftp untuk mengetahui struktur yang ada.

<img width="1918" height="714" alt="Cuplikan layar 2025-09-24 183909" src="https://github.com/user-attachments/assets/57ba4f2b-226c-43be-8550-2c6a3a47dff3" />

2. **Masuk ke easter egg**

setelah itu, masuk ke easter egg lalu tambahkan **%2500.md** untuk menginstall file.

<img width="1918" height="768" alt="Cuplikan layar 2025-09-24 184021" src="https://github.com/user-attachments/assets/73647b80-1872-4901-80f6-12e09b80ed3d" />

3. **Buka file easter egg yang telah didownload**

Pada file ini ditemukan kalimat yang terenkripsi dan kita diminta memecahkannya.

<img width="1168" height="501" alt="Cuplikan layar 2025-09-24 184049" src="https://github.com/user-attachments/assets/c1d9bb1c-dbce-41ec-99a4-bf7a44a0220c" />

4. **Dekode Base64**

Dekode Awal:

Tantangan ini memberikan string Base64:

```bash
L2d1ci9xcmlmL25lci9mYi9zaGFhbC9ndXJsL3V2cS9uYS9ybmZncmUvcnR0L2p2Z3V2YS9ndXIvcm5mZ3JlL3J0dA==
```

Dekode string tersebut menggunakan Base64 decoder menghasilkan:

```bash
/gur/qrif/ner/fb/shaal/gurl/uvq/na/rnfgre/rtt/jvguva/gur/rnfgre/rtt
```

<img width="1209" height="780" alt="Cuplikan layar 2025-09-24 184124" src="https://github.com/user-attachments/assets/e2cf158d-15c2-40e8-ad47-ee34abc58cfb" />

5. **Terapkan Cipher ROT13**

Dekripsi ROT13:

Hasil dekode tampak seperti acak, menunjukkan adanya cipher sederhana.
Gunakan ROT13 untuk mendekripsi sehingga menjadi:

```bash
/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg
```

<img width="1552" height="576" alt="Cuplikan layar 2025-09-24 184216" src="https://github.com/user-attachments/assets/3042406b-25e9-42ff-ab42-43ed349fd0d4" />

6. **Akses Nested Easter Egg**

Gunakan path hasil dekripsi untuk menavigasi ke:

```bash
localhost:3000/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg
```

<img width="1918" height="964" alt="Cuplikan layar 2025-09-24 184245" src="https://github.com/user-attachments/assets/96ca3036-42c9-42b7-bb66-1b7873692bae" />

Berhasil mengakses Easter egg yang sebenarnya dan menyelesaikan tantangan.

## Penjelasan Solusi

Tantangan ini merupakan latihan dekripsi yang memerlukan pemikiran teknis dan kreatif. Dimulai dengan string yang di-encode Base64, lalu didekode menjadi pesan yang tampak acak, dan kemudian didekripsi lagi dengan cipher ROT13 untuk menemukan Easter egg tersembunyi.

Tantangan ini menunjukkan bagaimana penggunaan beberapa lapisan pengaburan (multi-layered obfuscation) dapat meningkatkan tingkat kesulitan akses tidak sah.

## Rekomendasi Perbaikan

Untuk melindungi data dari dekripsi tidak sah:

Multi-factor Encryption: Gunakan lapisan enkripsi ganda dengan kunci atau metode berbeda untuk meningkatkan keamanan.

Best Practices Enkripsi: Gunakan algoritma yang kuat, tidak standar, dan selalu perbarui metode enkripsi secara berkala.

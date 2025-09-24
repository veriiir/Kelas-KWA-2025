# Write-up: Unlock Premium Challenge

## Overview

Judul: Premium Paywall

Kategori: Cryptographic Issues

Tantangan ini mengharuskan kita menemukan dan memanfaatkan kunci enkripsi untuk membuka bagian “premium content” OWASP Juice Shop tanpa melakukan pembayaran yang sebenarnya, mirip seperti bypass paywall pada aplikasi atau game.

<img width="428" height="200" alt="Cuplikan layar 2025-09-25 002301" src="https://github.com/user-attachments/assets/02f2f2bd-0505-4250-81fe-0fc70e415122" />

## Alat yang Digunakan

**Web Browser** – untuk menavigasi Juice Shop dan mengunduh file.

**Encryption key directory** – untuk memperoleh kunci dekripsi yang diperlukan.

**OpenSSL** – untuk mendekripsi string terenkripsi.

## Metodologi dan Solusi

1. **Menemukan Kunci Enkripsi&&

Dengan bantuan enumerasi direktori (misalnya Gobuster), ditemukan direktori:

```bash
localhost:3000/encryptionkeys
```

<img width="1915" height="404" alt="Cuplikan layar 2025-09-24 235321" src="https://github.com/user-attachments/assets/cfcbf819-ebdd-4e59-82e9-ec1e4d80de6f" />

Di dalamnya terdapat file premium.key yang kemudian diunduh.

2. **Menganalisis Isi Kunci**

Isi file:

```bash
1337133713371337.EA99A61D92D2955B1E9285B55BF2AD42
```

<img width="786" height="282" alt="Cuplikan layar 2025-09-24 235438" src="https://github.com/user-attachments/assets/bcc9ec90-09a0-47e7-8d1f-ae94dd088f4b" />

Bagian pertama terlihat seperti IV, sedangkan bagian kedua seperti kunci heksadesimal untuk AES (128/256 bit).

3. **Menemukan Data Terenkripsi**

Tantangan mengisyaratkan adanya mekanisme “payment” tersembunyi. Dengan menelusuri komentar sumber halaman, ditemukan string terenkripsi berbentuk Base64 di dalam HTML:

```bash
<!--IvLuRfBJYlmStf9XfL6ckJFngyd9LfV1JaaN/KRTPQPidTuJ7FR+D/nkWJUF+0xUF07CeCeqYfxq+OJVVa0gNbqgYkUNvn//UbE7e95C+6e+7GtdpqJ8mqm4WcPvUGIUxmGLTTAC2+G9UuFCD1DUjg==-->
```

4. **Mendekripsi URL Tersembunyi**

Menggunakan OpenSSL dengan kunci dan IV dari premium.key, string tersebut didekripsi:

```bash
echo "IvLuRfBJYlmStf9XfL6ckJFngyd9LfV1JaaN/KRTPQPidTuJ7FR+D/nkWJUF+0xUF07CeCeqYfxq+OJVVa0gNbqgYkUNvn//UbE7e95C+6e+7GtdpqJ8mqm4WcPvUGIUxmGLTTAC2+G9UuFCD1DUjg==" \
| openssl enc -d -aes-256-cbc -K EA99A61D92D2955B1E9285B55BF2AD42 -iv 1337133713371337 -a -A
```

Hasil dekripsi menampilkan URL tersembunyi di balik paywall:

```bash
http://localhost:3000/this/page/is/hidden/behind/an/incredibly/high/paywall/that/could/only/be/unlocked/by/sending/1btc/to/us
```

5. **Mengakses Konten Premium**

Mengakses URL hasil dekripsi langsung membuka halaman premium content Juice Shop tanpa pembayaran – tantangan berhasil diselesaikan.

<img width="1913" height="973" alt="Cuplikan layar 2025-09-25 002408" src="https://github.com/user-attachments/assets/48933277-1c52-4d8a-96bc-edb1526538a5" />

## Penjelasan Solusi

Challenge ini terselesaikan dengan memanfaatkan kunci enkripsi yang dibiarkan terbuka di direktori server. Kunci tersebut memungkinkan dekripsi URL yang mengarah ke halaman “premium”, sehingga paywall dapat dilewati. Ini menunjukkan bahaya besar jika kunci atau rahasia disimpan di lokasi publik.

# Write-up: User ID controlled by request parameter

## Overview

Title: User ID controlled by request parameter

Category: Broken Access Control

Decribe: This lab has a horizontal privilege escalation vulnerability on the user account page. To solve the lab, obtain the API key for the user carlos and submit it as the solution. You can log in to your own account using the following credentials: wiener:peter

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter)

## Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**

<img width="1919" height="969" alt="Cuplikan layar 2025-09-17 143854" src="https://github.com/user-attachments/assets/ac9557df-3308-4a9f-afba-ca234ee033c4" />

2. **Lakukan Login dengan username dan password yang sudah disertakan**

Sebelum memulai mengerjakan, kita diberikan kredensial username dan password yaitu *wiener:peter* untuk login.

<img width="1919" height="970" alt="Cuplikan layar 2025-09-17 143934" src="https://github.com/user-attachments/assets/32e3e342-f408-4920-8a8d-62d9f68b5eb3" />

3. **Maka akan menampilkan user beserta API yang diminta**

Setelah Login, maka akan menampilkan user beserta API yang diminta.

<img width="1919" height="968" alt="Cuplikan layar 2025-09-17 143943" src="https://github.com/user-attachments/assets/6f3f781e-a14f-4cb9-b32e-c33108d03e99" />

4. **Ubah id user pada URL**

Lakukan perubahan pada id user yang awalnya *wiener* menjadi *carlos* untuk mendapatkan API yang diminta.

<img width="1919" height="972" alt="Cuplikan layar 2025-09-17 143959" src="https://github.com/user-attachments/assets/a3142f77-c78a-451a-aa00-56676ddc7ced" />

5. **Masukkan API yang diminta kedalam submission yang telah disediakan**

 Setelah mendapatkan API user Carlos, kita dapat memasukkan API tersebut untuk menyelesaikan soal ini.

<img width="1919" height="970" alt="Cuplikan layar 2025-09-17 144010" src="https://github.com/user-attachments/assets/24b0fb6e-bf9f-47a5-9737-e1c91132d1d0" />

Hasil:

<img width="1919" height="972" alt="Cuplikan layar 2025-09-17 144019" src="https://github.com/user-attachments/assets/d763372a-2d0f-4927-92eb-d382f1c52372" />

# Write-up: User ID controlled by request parameter with password disclosure

## Overview

Title: User ID controlled by request parameter with password disclosure

Category: Broken Access Control

Decribe: This lab has user account page that contains the current user's existing password, prefilled in a masked input. To solve the lab, retrieve the administrator's password, then use it to delete the user carlos. You can log in to your own account using the following credentials: wiener:peter

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure)

## Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**

<img width="1919" height="973" alt="Cuplikan layar 2025-09-17 145312" src="https://github.com/user-attachments/assets/9371cf3f-df9a-4569-82d6-3cd08a0bae36" />

2. **Lakukan Login dengan username dan password yang sudah disertakan**

Sebelum memulai mengerjakan, kita diberikan kredensial username dan password yaitu *wiener:peter* untuk login.

<img width="1917" height="969" alt="Cuplikan layar 2025-09-17 145423" src="https://github.com/user-attachments/assets/a67a755b-3458-4feb-8e4b-19eaac25923b" />

3. **Ubah id user pada URL**

Setelah Login, tampilan menampilkan email serta password jika ingin diubah. lalu ubah id user yang awalnya *wiener* menjadi *dministrator*. kemudian lakukan inspect untuk mengetahui password administrator agar bisa login sebagai admin.
<img width="1919" height="970" alt="Cuplikan layar 2025-09-17 145553" src="https://github.com/user-attachments/assets/df74508e-710a-4b2b-b6f6-365e84d05e04" />
<img width="1919" height="970" alt="Cuplikan layar 2025-09-17 145621" src="https://github.com/user-attachments/assets/e9d57e4f-cc17-42f5-a280-056b94026678" />

4. **Logout sebagai User Wiener dan Login kembali sebagai Administrator**

Lakukan Login ulang dengan username *Administrator* dan Password yang telah didapatkan.

<img width="1919" height="970" alt="Cuplikan layar 2025-09-17 145700" src="https://github.com/user-attachments/assets/9d0f7940-1bef-428b-8284-df9a419e63b8" />

5. **Menghapus User Carlos seperti yang diminta**

 Setelah berhasil login, masuk ke admin-panel untuk menghapus user carlos.

<img width="1919" height="912" alt="Cuplikan layar 2025-09-17 145719" src="https://github.com/user-attachments/assets/b12f6ff8-36fb-4235-8474-b70108b16b43" />

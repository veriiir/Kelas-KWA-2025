# Write-up: Unprotected admin functionality

## Overview

Title: Unprotected admin functionality

Category: Broken Access Control

Decribe: deleting the user carlos.

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality)

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**

   <img width="1919" height="970" alt="Cuplikan layar 2025-09-16 232915" src="https://github.com/user-attachments/assets/ef38a72b-46e2-4e8e-ac9b-185b6440a3f2" />

3. **Setelah masuk ke halaman login. Masukkan kata */admin* pada url halaman web**

   <img width="1919" height="481" alt="Cuplikan layar 2025-09-17 134120" src="https://github.com/user-attachments/assets/882436f3-0274-4328-9619-b537964add35" />

   setelah menambahkan */admin* pada url soal, diketahui bahwa akses tidak ditemukan.
   
5. **Lakukan check pada file *robots.txt***

   Disini kita dapat melakukan pengecekan file *robots.txt* untuk mengetahui direktori tersembunyi.

   <img width="1917" height="405" alt="Cuplikan layar 2025-09-17 134139" src="https://github.com/user-attachments/assets/253d7d52-ab87-4c01-a2ed-a3b262b9a99c" />

7. **Masuk ke dalam administrator-panel untuk mengetahui user yang ada**

   setelah mendapatkan direktori tersembunyi, masuk ke direktori tersebut untuk mengetahui user-user yang ada.

   <img width="1917" height="701" alt="Cuplikan layar 2025-09-17 134225" src="https://github.com/user-attachments/assets/ff5841a1-8ba6-4163-8ba0-4b1a8100bedd" />

8. **Hapus User Carlos**

   Setelah mendapatkan User *Carlos*, maka lakukan penghapusan user sesuai dengan yang diminta.

   <img width="1919" height="735" alt="Cuplikan layar 2025-09-17 134236" src="https://github.com/user-attachments/assets/6b41c667-75bf-4505-8bde-2fd2166f2be4" />

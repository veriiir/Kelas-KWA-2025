# Write-up: Unprotected admin functionality with unpredictable URL

## Overview

Title: Unprotected admin functionality with unpredictable URL

Category: Broken Access Control

Decribe: This lab has an unprotected admin panel. It's located at an unpredictable location, but the location is disclosed somewhere in the application. Solve the lab by accessing the admin panel, and using it to delete the user carlos.

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url)

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**

   <img width="1919" height="963" alt="Cuplikan layar 2025-09-17 142251" src="https://github.com/user-attachments/assets/d668539d-752f-4489-952b-a7d85574b282" />

2. **Masuk ke direktori Admin-panel**

   Disini kita dapat masuk ke direktori admin-panel, tetapi karena tidak ada akses menyebebkan not found.

   <img width="1919" height="209" alt="Cuplikan layar 2025-09-17 142307" src="https://github.com/user-attachments/assets/5bc50fd7-0e04-47a1-a936-e7dc3e67b1c7" />

3. **Lakukan Ctrl+U agar menampilkan source codenya**

   Disini kita lakukan short cut *ctrl+U* untuk menampilkan source code halaman soal.

4. **Cari Admin-Panel tag yang berguna untuk memasuki direktori admin**

   setelah berhasil menampilkan source codenya, maka lakukan pencarian mengenai tag admin-panel untuk memasuki direktori admin.

   <img width="1919" height="956" alt="Cuplikan layar 2025-09-17 142333" src="https://github.com/user-attachments/assets/41e360df-bd91-45e0-86f7-bced972c2921" />

6. **Masuk ke direktori admin-panel dengan tag */admin-8twy8q* yang telah didapat**

   <img width="1916" height="972" alt="Cuplikan layar 2025-09-17 142354" src="https://github.com/user-attachments/assets/f26f3680-0257-4a2c-8dd6-c9e979fd4b46" />

8. **Hapus User Carlos**

   Setelah mendapatkan User _Carlos_, maka lakukan penghapusan user sesuai dengan yang diminta.

   <img width="1919" height="969" alt="Cuplikan layar 2025-09-17 142403" src="https://github.com/user-attachments/assets/d33856be-4be1-42e2-a1d0-7009395fc1f4" />

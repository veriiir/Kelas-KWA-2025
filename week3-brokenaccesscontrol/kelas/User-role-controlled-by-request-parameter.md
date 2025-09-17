# Write-up: User role controlled by request parameter

## Overview

Title: User role controlled by request parameter

Category: Broken Access Control

Decribe: This lab has an admin panel at /admin, which identifies administrators using a forgeable cookie.

Solve the lab by accessing the admin panel and using it to delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

### Alat yang Digunakan

- **Browser:** Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.
- **Developer Tools** Digunakan untuk mengubah parameter.

### Resource

Soal: [course portswigger](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter)

#### Langkah-Langkah yang Diambil untuk Menyelesaikan Tantangan

1. **Masuk Halaman Web terlebih dahulu**

   <img width="1919" height="967" alt="Cuplikan layar 2025-09-17 140328" src="https://github.com/user-attachments/assets/848b9aa9-9e23-4faf-9a63-f2c666aa4f7e" />

3. **Lakukan Login dengan username dan password yang sudah disertakan**

   Sebelum memulai mengerjakan, kita diberikan kredensial username dan password yaitu wiener:peter. kemudian lakukan login

   <img width="1919" height="964" alt="Cuplikan layar 2025-09-17 140348" src="https://github.com/user-attachments/assets/b838bbbb-f45f-497d-8180-a27323a338c0" />

5. **Masuk ke direktori Admin**

   Disini kita dapat masuk ke direktori admin, tetapi karena login bukan sebagai admin maka tidak bisa mengaksesnya.

   <img width="1908" height="968" alt="Cuplikan layar 2025-09-17 140500" src="https://github.com/user-attachments/assets/a85336e7-c6ed-4e2e-ad59-8573305a3436" />

7. **Gunakan Developer Tools untuk mengubah parameter**

   Disini kita menggunakan developer Tools untuk mengubah parameter pada cookie yang awalnya *false* menjadi *true* untuk bisa memasuki direktori admin.

   <img width="1919" height="974" alt="Cuplikan layar 2025-09-17 140631" src="https://github.com/user-attachments/assets/45628ff5-c222-4fa9-aa42-8ad0bee8c14f" />

9. **Masuk ke dalam direktori admin untuk mengetahui user yang ada**

   setelah mengubah parameter, sekarang dapat masuk ke direktori admin untuk mengetahui user-user yang ada.

   <img width="1917" height="964" alt="Cuplikan layar 2025-09-17 140718" src="https://github.com/user-attachments/assets/022e408d-1b8e-42b3-a066-dcf0aecd4be0" />

5. **Hapus User Carlos**

   Setelah mendapatkan User _Carlos_, maka lakukan penghapusan user sesuai dengan yang diminta.

   <img width="1919" height="965" alt="Cuplikan layar 2025-09-17 140728" src="https://github.com/user-attachments/assets/a90b71a4-7531-4c1d-b0f7-424280ea7f36" />

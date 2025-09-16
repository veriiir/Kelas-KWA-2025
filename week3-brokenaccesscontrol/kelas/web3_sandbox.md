# Write-up: Web3 Sandbox

## Overview

Judul: Web3 Sandbox

Kategori: Broken Access Control

Penjelasan: Tantangan ini mengharuskan penemuan sebuah Web3 code sandbox yang secara tidak sengaja dideploy dan memungkinkan penulisan serta pengujian smart contract secara langsung (on-the-fly). Tantangan ini mengeksplorasi konsep broken access control pada aplikasi web, khususnya pada lingkungan yang melibatkan teknologi Web3.

<img width="440" height="243" alt="Cuplikan layar 2025-09-11 142228" src="https://github.com/user-attachments/assets/053b3538-ae66-4d01-a71f-679d2ac257e3" />

## Alat yang Digunakan

**Browser web**: Digunakan untuk berinteraksi dengan antarmuka masuk (login) dari aplikasi web.

## Metodologi dan Solusi

1. **Penemuan**

Dimulai dengan menjelajahi berbagai URL yang berkaitan dengan fungsionalitas Web3 pada aplikasi, dengan kecurigaan bahwa sandbox tersebut mungkin berada di path yang agak tersembunyi atau tidak jelas.

2. **Menebak URL atau Mencari Source file yang berisi path**

Menggunakan pendekatan penebakan sederhana, terinspirasi dari nama direktori umum yang sering dipakai untuk testing atau lingkungan pengembangan seperti /login, /web3, /sandbox, dan sejenisnya. Pada kasus ini, Saya menggunakan inspect untuk membaca source file main nya, sehingga menemukan path web3-sandbox:

<img width="1919" height="980" alt="Cuplikan layar 2025-09-11 142402" src="https://github.com/user-attachments/assets/55450cd0-99ac-4bc3-8cde-f08ba87b572b" />

```bash
https://juice-shop.herokuapp.com/#/web3-sandbox
```

3. **Mengakses Sandbox**

Setelah membuka URL yang tepat, ditemukan sebuah lingkungan Web3 code sandbox yang berfungsi penuh. Sandbox ini menyediakan fitur untuk mengedit, meng-compile, dan men-deploy smart contract Ethereum langsung dari browser.

<img width="1919" height="968" alt="Cuplikan layar 2025-09-11 142446" src="https://github.com/user-attachments/assets/edb205e7-7f5b-4a36-8f94-afad6ca6b27e" />

## Penjelasan Solusi

Tantangan ini mengeksploitasi kelalaian keamanan yang umum, yaitu ketika alat pengembangan atau fitur eksperimental secara tidak sengaja dibiarkan dapat diakses di lingkungan produksi. Dengan mengakses URL tersebut secara langsung, pengguna memperoleh akses tak terbatas ke fungsionalitas berbahaya yang seharusnya hanya digunakan dalam lingkungan terkontrol, sehingga menimbulkan risiko keamanan yang signifikan.

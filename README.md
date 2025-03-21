<br>
<div align="center">
<img alt="LOGO" src="https://raw.githubusercontent.com/bangtutorial/streamflow/refs/heads/main/public/img/logo.png" width="420" height="auto" />
</div>
<br>



# StreamFlow: Aplikasi Live Streaming Multi-Window

StreamFlow adalah aplikasi live streaming yang memungkinkan kamu untuk melakukan live streaming ke berbagai platform seperti YouTube, Facebook, dan lainnya menggunakan protokol RTMP. Aplikasi ini berjalan di VPS (Virtual Private Server) dan mendukung streaming ke banyak platform sekaligus dengan fitur multi-window. StreamFlow juga dilengkapi dengan fitur login, gallery dan history streaming untuk melacak history streaming.
   <p align="center">
   <img alt="screenshot" src="https://raw.githubusercontent.com/bangtutorial/streamflow/refs/heads/main/public/img/screenshot.jpg" width="500px" height="auto" />
   </p>

## Fitur Utama:

* **Multi-Window Streaming:** Bisa melakukan streaming ke beberapa platform sekaligus dalam satu aplikasi.
* **Dukungan Banyak Platform:** Bisa streaming ke YouTube, Facebook, dan platform lain yang mendukung RTMP.
* **Login Page:** Ada fitur login supaya hanya pemilik akun yang bisa akses aplikasi.
* **Riwayat Streaming:** Semua aktivitas streaming tersimpan, jadi bisa dilihat kembali kapan saja.

## Cara Instalasi:

**Sebelum mulai:** Pastikan server / VPS kamu sudah terinstall Node.js, npm, dan FFmpeg sebelum meng-clone repositori ini.

1. **Install Node.js dan npm melalui NodeSource PPA:**

   ```bash
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install -y npm
   ```
   Cek apakah instalasi berhasil:
   ```bash
   node -v
   npm -v
   ```

2. **Install FFmpeg:**

   ```bash
   sudo apt-get update
   sudo apt-get install -y ffmpeg
   ```
   Cek apakah instalasi berhasil:
   ```bash
   ffmpeg -version
   ```
   
3. **Install PM2 + Sharp:**

   ```bash
   npm install -g pm2
   npm install --os=linux --cpu=x64 sharp
   ```

4. **Clone Repositori:**
   ```bash
   git clone https://github.com/bangtutorial/streamflow/
   cd streamflow
   ```

5. **Install Dependensi:**
   Jalankan `npm install` untuk menginstal semua modul Node.js yang dibutuhkan seperti Express.js, SQLite3, bcryptjs, dan lainnya.

   ```bash
   npm install
   ```

6. **Jalankan Aplikasi:**

   Kembali ke directory root (jika masih di directory streamflow)
   ```bash
   cd ..
   ```

   🚀 Perintah menjalankan aplikasi ✨
   ```bash
   pm2 start streamflow
   pm2 logs streamflow -i 0 --lines 1
   ```

   📈 Melihat status aplikasi berjalan
   ```bash
   pm2 status streamflow
   ```

   ⛔ Menghentikan aplikasi
   ```bash
   pm2 stop streamflow
   ```

7. **Reset Password:**
   
   Jalankan perintah ini di terminal
   ```bash
   npm start reset-streamflow
   ```

9. **Konfigurasi:**
    * Pastikan kamu sudah mengatur URL RTMP yang sesuai untuk setiap platform yang ingin digunakan. Konfigurasi ini bisa dilakukan langsung melalui tampilan aplikasi.
    * Silahkan dapatkan Stream Key dari platform streaming yang kamu gunakan.

Untuk memastikan aplikasi **StreamFlow** berjalan otomatis setelah reboot di VPS lain, kita perlu menambahkan skrip agar PM2 dapat memulai aplikasi tersebut secara otomatis. Kita akan menggunakan systemd untuk memastikan aplikasi berjalan saat VPS restart.

Berikut adalah langkah-langkah yang perlu ditambahkan ke file `README.html` yang kamu miliki.

### 1. **Membuat Skrip Systemd untuk PM2:**

Kita akan membuat file service systemd agar PM2 dapat berjalan otomatis setelah reboot.

**Langkah pertama: Buat file systemd untuk PM2**

1. Jalankan perintah berikut untuk membuat file `pm2-streamflow.service`:
   
   ```bash
   sudo nano /etc/systemd/system/pm2-streamflow.service
   ```

2. Salin dan tempel skrip berikut di file `pm2-streamflow.service`:

   ```bash
   [Unit]
   Description=PM2 process manager for StreamFlow app
   Documentation=https://pm2.keymetrics.io/
   After=network.target

   [Service]
   Type=forking
   User=root
   LimitNOFILE=infinity
   LimitNPROC=infinity
   LimitCORE=infinity
   Environment=PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
   Environment=PM2_HOME=/root/.pm2
   PIDFile=/root/.pm2/pm2.pid
   Restart=always
   ExecStart=/usr/lib/node_modules/pm2/bin/pm2 start /root/streamflow/src/server.js --name "streamflow"
   ExecReload=/usr/lib/node_modules/pm2/bin/pm2 reload all
   ExecStop=/usr/lib/node_modules/pm2/bin/pm2 stop all

   [Install]
   WantedBy=multi-user.target
   ```

3. Simpan dan tutup file (`Ctrl+X`, lalu tekan `Y` untuk menyimpan, dan `Enter`).

### 2. **Reload systemd dan Enable Service:**

1. Reload systemd agar file service yang baru saja dibuat dikenali:

   ```bash
   sudo systemctl daemon-reload
   ```

2. Enable service agar dapat berjalan otomatis setelah reboot:

   ```bash
   sudo systemctl enable pm2-streamflow
   ```

3. Jalankan service untuk memulai PM2 dan aplikasi StreamFlow:

   ```bash
   sudo systemctl start pm2-streamflow
   ```

### 3. **Verifikasi PM2 dan Service Systemd:**

1. Cek status service `pm2-streamflow` untuk memastikan berjalan dengan baik:

   ```bash
   sudo systemctl status pm2-streamflow
   ```

2. Cek apakah aplikasi StreamFlow sudah berjalan melalui PM2:

   ```bash
   pm2 list
   ```

3. Setelah reboot, aplikasi akan dijalankan otomatis.

### 4. **Test Setelah Reboot:**

Lakukan reboot untuk memastikan semuanya berjalan otomatis:

```bash
sudo reboot
```

Setelah VPS reboot, login kembali dan periksa status PM2:

```bash
pm2 list
```

Itu dia! Aplikasi StreamFlow sekarang sudah terkonfigurasi untuk berjalan otomatis setelah reboot.

## Informasi Tambahan:

* Aplikasi ini menggunakan Express.js sebagai backend, SQLite sebagai database, dan FFmpeg untuk encoding serta streaming.
* Antarmuka pengguna dibuat dengan HTML, CSS, dan JavaScript, serta menggunakan Tailwind CSS untuk styling.
* Aplikasi ini dirancang untuk berjalan di server dengan Node.js, bukan di browser lokal.

## Kontribusi:

Jika teman-teman punya ide atau perbaikan koding aplikasi ini, silakan buat pull request 🤝

## Lisensi:

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://github.com/bangtutorial/streamflow/blob/main/LICENSE)

© 2025 - [Bang Tutorial](https://youtube.com/bangtutorial)

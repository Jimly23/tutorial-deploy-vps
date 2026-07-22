# 🚀 VPS Deployment Documentation

<p align="center">
  <img src="https://img.shields.io/badge/Linux-Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white"/>
  <img src="https://img.shields.io/badge/Nginx-Web%20Server-009639?style=for-the-badge&logo=nginx&logoColor=white"/>
  <img src="https://img.shields.io/badge/PHP-8.x-777BB4?style=for-the-badge&logo=php&logoColor=white"/>
  <img src="https://img.shields.io/badge/Laravel-Framework-FF2D20?style=for-the-badge&logo=laravel&logoColor=white"/>
  <img src="https://img.shields.io/badge/Next.js-Framework-black?style=for-the-badge&logo=nextdotjs"/>
  <img src="https://img.shields.io/badge/React-Vite-61DAFB?style=for-the-badge&logo=react"/>
</p>

---

# 📖 Daftar Isi

- [Hosting Laravel API](#-hosting-api-laravel)
- [Menghapus Project Laravel](#-menghapus-project-laravel)
- [Troubleshooting Laravel](#-troubleshooting-laravel)
- [Hosting Frontend React (Vite)](#-hosting-frontend-react-vite)
- [Hosting Next.js](#-hosting-nextjs)
- [Hosting CodeIgniter](#-hosting-codeigniter)

---

# 🚀 Hosting API Laravel

## 📋 Prasyarat

Pastikan VPS telah memiliki:

- ✅ MySQL
- ✅ Composer
- ✅ Nginx
- ✅ PHP
- ✅ phpMyAdmin

---

# 📁 1. Membuat Folder Project

Masuk ke folder web server.

```bash
cd /var/www
```

Buat folder project.

```bash
sudo mkdir nama_folder
```

Masuk ke folder tersebut.

```bash
cd nama_folder
```

---

# 📥 2. Clone Repository

Clone project Laravel dari GitHub.

```bash
sudo git clone https://github.com/username/project.git
```

---

# ⚙️ 3. Konfigurasi File .env

Copy file environment.

```bash
cp .env.example .env
```

Edit file.

```bash
nano .env
```

Sesuaikan konfigurasi berikut:

```env
DB_DATABASE=nama_database
DB_USERNAME=username_database
DB_PASSWORD=password_database
```

Simpan perubahan.

```
CTRL + O
ENTER
CTRL + X
```

---

# 👤 4. Mengubah Ownership

Jika menggunakan user selain root.

```bash
sudo chown -R ubuntu:ubuntu /var/www/nama_project
```

---

# 🔐 5. Mengatur Permission

```bash
chmod -R 775 storage bootstrap/cache
```

---

# 📦 6. Install Dependency

```bash
composer install --no-dev --optimize-autoloader
```

---

# 🗄️ 7. Membuat Database

Masuk ke phpMyAdmin.

```
http://IP_VPS/phpMyAdmin
```

Buat database baru sesuai nama pada file `.env`.

---

# 🔑 8. Generate Laravel Key

```bash
php artisan key:generate
```

---

# 🔗 9. Membuat Storage Link

```bash
php artisan storage:link
```

---

# 🛠️ 10. Menjalankan Migration

```bash
php artisan migrate
```

---

# 🌐 11. Konfigurasi Nginx

Buat file konfigurasi.

```bash
sudo nano /etc/nginx/sites-available/nama_file
```

Isi dengan konfigurasi berikut.

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name yapi.bitblock.my.id;

    root /var/www/api.youvitation.com/html/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico {
        access_log off;
        log_not_found off;
    }

    location = /robots.txt {
        access_log off;
        log_not_found off;
    }

    error_page 404 /index.php;

    location ~ ^/index\.php(/|$) {

        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;

        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;

        include fastcgi_params;

        fastcgi_hide_header X-Powered-By;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

---

# 🔗 12. Mengaktifkan Site

Buat symbolic link.

```bash
sudo ln -s /etc/nginx/sites-available/nama_file /etc/nginx/sites-enabled
```

---

# 🔄 13. Reload Nginx

Cek konfigurasi.

```bash
sudo nginx -t
```

Reload nginx.

```bash
sudo systemctl reload nginx
```

---

# 🔒 14. Mengatur Permission Storage

```bash
sudo chown -R www-data:www-data /var/www/youvitation-game/stg-backend/storage

sudo chown -R www-data:www-data /var/www/youvitation-game/stg-backend/bootstrap/cache

sudo chmod -R 775 /var/www/youvitation-game/stg-backend/storage

sudo chmod -R 775 /var/www/youvitation-game/stg-backend/bootstrap/cache
```

---

# 📝 15. Membuat Session Table (Jika Error)

```bash
php artisan session:table
```

---

# 🔐 16. Memasang SSL

```bash
sudo certbot --nginx -d stg-api.youvitation.net
```

---

## ✅ Deployment Laravel Selesai

Jika semua langkah berhasil dilakukan maka API Laravel sudah dapat diakses menggunakan domain yang telah dikonfigurasi.

---

> 💡 **Tips**
>
> Jangan pernah menjalankan aplikasi Laravel menggunakan user `root`. Gunakan user deployment agar permission lebih aman dan lebih mudah dikelola.

---

---

# 🗑️ Menghapus Project Laravel di VPS

Jika project Laravel sudah tidak digunakan, lakukan langkah-langkah berikut untuk menghapus seluruh konfigurasi yang terkait di VPS.

> ⚠️ **Peringatan**
>
> Langkah-langkah berikut akan menghapus project beserta konfigurasi Nginx dan database secara permanen. Pastikan Anda telah melakukan backup apabila masih diperlukan.

---

## 📁 1. Menghapus Folder Project

Masuk ke direktori project (opsional untuk memastikan lokasi).

```bash
cd /var/www
```

Hapus folder project.

```bash
sudo rm -rf nama_projek
```

> Contoh:

```bash
sudo rm -rf api.youvitation.com
```

---

## 🌐 2. Menghapus Konfigurasi Nginx

Hapus file symbolic link pada folder `sites-enabled`.

```bash
sudo rm /etc/nginx/sites-enabled/nama_projek
```

Hapus file konfigurasi pada folder `sites-available`.

```bash
sudo rm /etc/nginx/sites-available/nama_projek
```

---

## 🔄 3. Reload Nginx

Pastikan konfigurasi Nginx masih valid.

```bash
sudo nginx -t
```

Apabila tidak terdapat error, reload Nginx.

```bash
sudo systemctl reload nginx
```

Jika muncul pesan seperti berikut, berarti konfigurasi berhasil.

```text
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

---

## 🗄️ 4. Menghapus Database MySQL

Masuk ke MySQL menggunakan user `root`.

```bash
sudo mysql -u root -p
```

Masukkan password MySQL.

---

### 📋 Menampilkan Seluruh Database

```sql
SHOW DATABASES;
```

Contoh output:

```text
+----------------------+
| Database             |
+----------------------+
| information_schema   |
| mysql                |
| performance_schema   |
| nama_database        |
+----------------------+
```

---

### ❌ Menghapus Database

```sql
DROP DATABASE nama_database;
```

Contoh:

```sql
DROP DATABASE youvitation_db;
```

---

### 🚪 Keluar dari MySQL

```sql
EXIT;
```

---

## ✅ Project Berhasil Dihapus

Apabila seluruh langkah di atas telah dilakukan, maka:

- ✅ Folder project telah dihapus.
- ✅ Konfigurasi Nginx telah dihapus.
- ✅ Domain tidak lagi mengarah ke project tersebut.
- ✅ Database MySQL telah dihapus.
- ✅ VPS telah bersih dari project Laravel tersebut.

> 💡 **Tips**
>
> Sebelum menghapus project, disarankan untuk melakukan backup source code dan database agar data tetap dapat dipulihkan apabila diperlukan di kemudian hari.

---

---

# ⚛️ Hosting Frontend React (Vite) di VPS

Dokumentasi ini digunakan untuk melakukan deployment aplikasi frontend React (Vite) menggunakan hasil build (`dist`) ke VPS dengan web server **Nginx**.

> 📌 **Catatan**
>
> Tutorial ini menggunakan metode deployment **Static Build** (`npm run build`), sehingga hanya folder `dist` yang akan di-upload ke VPS.

---

## 📦 1. Build Project

Jalankan proses build pada komputer lokal.

```bash
npm run build
```

Setelah proses selesai, akan terbentuk folder:

```text
dist/
```

Folder inilah yang nantinya akan di-upload ke VPS.

---

## 🔑 2. Login ke VPS

Masuk ke VPS menggunakan SSH.

```bash
ssh username@IP_VPS
```

Contoh:

```bash
ssh jimlyassidqi@46.202.135.131
```

---

## 📁 3. Membuat Folder Project

Masuk ke direktori web server.

```bash
cd /var/www
```

Buat folder project.

```bash
sudo mkdir -p nama_projek
```

Contoh:

```bash
sudo mkdir -p frontend
```

---

## 👤 4. Mengubah Ownership Folder

Agar user deployment memiliki hak akses terhadap folder project.

```bash
sudo chown -R ubuntu:ubuntu /var/www/nama_projek
```

Contoh:

```bash
sudo chown -R ubuntu:ubuntu /var/www/youvitation-game/frontend
```

---

## 📤 5. Upload Folder Build ke VPS

Jalankan perintah berikut **dari komputer lokal**, bukan dari VPS.

```bash
scp -r ./dist/* username@IP_VPS:/var/www/nama_projek
```

Contoh:

```bash
scp -r ./dist/* ubuntu@43.134.1.60:/var/www/youvitation-game/frontend
```

> 💡 **Penjelasan**
>
> Perintah `scp` digunakan untuk mengirim seluruh isi folder `dist` ke direktori project di VPS.

---

## 🌐 6. Konfigurasi Nginx

Buat file konfigurasi baru.

```bash
sudo nano /etc/nginx/sites-available/nama_projek
```

Tambahkan konfigurasi berikut.

```nginx
server {

    listen 80;

    server_name namadomain.com www.namadomain.com;

    root /var/www/nama_projek;

    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

}
```

### Penjelasan

- **server_name** → Domain yang digunakan.
- **root** → Lokasi folder hasil build (`dist`) di VPS.
- **try_files** → Memastikan React Router tetap dapat berjalan ketika halaman di-refresh.

---

## 🔗 7. Mengaktifkan Konfigurasi Nginx

Buat symbolic link dari `sites-available` ke `sites-enabled`.

```bash
sudo ln -s /etc/nginx/sites-available/nama_projek /etc/nginx/sites-enabled/
```

---

## 🔄 8. Reload Nginx

Pastikan konfigurasi Nginx tidak memiliki error.

```bash
sudo nginx -t
```

Apabila konfigurasi valid, restart Nginx.

```bash
sudo systemctl restart nginx
```

Jika berhasil akan muncul pesan seperti berikut.

```text
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

---

## 🔐 9. Mengatur Permission Folder

Berikan hak akses kepada user deployment.

```bash
sudo chown -R jimlyassidqi:jimlyassidqi /var/www/nama_projek
```

Kemudian ubah permission folder.

```bash
sudo chmod -R 755 /var/www/nama_projek
```

---

## ✅ Deployment Berhasil

Jika seluruh langkah di atas telah dilakukan, maka:

- ✅ Folder `dist` berhasil di-upload ke VPS.
- ✅ Nginx berhasil dikonfigurasi.
- ✅ Domain mengarah ke aplikasi React.
- ✅ React Router dapat berjalan tanpa error saat halaman di-refresh.
- ✅ Frontend siap diakses melalui browser.

---

> 💡 **Best Practice**
>
> Setiap kali ada perubahan pada project React, cukup lakukan langkah berikut:
>
> 1. Jalankan `npm run build` pada komputer lokal.
> 2. Upload ulang isi folder `dist` ke VPS menggunakan `scp`.
> 3. Tidak perlu melakukan restart aplikasi karena React (Vite) bersifat static website.

---

---

# ▲ Hosting Next.js di VPS

Dokumentasi ini menjelaskan cara melakukan deployment aplikasi **Next.js** ke VPS menggunakan **Nginx** sebagai Reverse Proxy dan **PM2** sebagai Process Manager.

> 📌 **Catatan**
>
> Tutorial ini menggunakan mode **Production** (`npm run build`) dan menjalankan aplikasi menggunakan **PM2** agar aplikasi tetap berjalan meskipun terminal SSH ditutup.

---

## 📁 1. Membuat Folder Project

Masuk ke direktori web server.

```bash
cd /var/www
```

Buat folder project.

```bash
sudo mkdir -p nama-folder
```

Masuk ke folder tersebut.

```bash
cd nama-folder
```

---

## 👤 2. Mengatur Ownership Folder

Berikan hak akses kepada user deployment.

```bash
sudo chown -R ubuntu:ubuntu /var/www/nama-folder
```

---

## 📥 3. Clone Repository

Clone project dari GitHub.

```bash
git clone https://github.com/username/project.git .
```

> Pastikan menjalankan perintah di dalam folder project yang telah dibuat.

---

## ⚙️ 4. Konfigurasi File `.env`

Copy file environment apabila diperlukan.

```bash
cp .env.example .env
```

Edit konfigurasi.

```bash
nano .env
```

Sesuaikan seluruh konfigurasi seperti:

- Database
- API URL
- Authentication
- Environment Variable lainnya

Simpan perubahan menggunakan:

```
CTRL + O
ENTER
CTRL + X
```

---

## 📦 5. Install Dependency

```bash
npm install
```

---

## 🏗️ 6. Build Project

Build aplikasi Next.js untuk mode production.

```bash
npm run build
```

---

## 🌐 7. Konfigurasi Nginx

Buat file konfigurasi.

```bash
sudo nano /etc/nginx/sites-available/nama_project
```

Tambahkan konfigurasi berikut.

```nginx
server {

    server_name youvitation.net www.youvitation.net;

    location / {

        proxy_pass http://127.0.0.1:3000;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

    }

}

server {

    listen 80;

    server_name youvitation.net www.youvitation.net;

    return 301 https://$host$request_uri;

}
```

---

## 🔗 8. Mengaktifkan Konfigurasi Nginx

Buat symbolic link.

```bash
sudo ln -s /etc/nginx/sites-available/nama_project /etc/nginx/sites-enabled/
```

Kemudian cek konfigurasi.

```bash
sudo nginx -t
```

Jika tidak terdapat error, reload Nginx.

```bash
sudo systemctl reload nginx
```

---

## ⚡ 9. Menjalankan Aplikasi Menggunakan PM2

Masuk ke folder project.

```bash
cd /var/www/nama-folder
```

### Melihat Aplikasi yang Sedang Berjalan

```bash
pm2 logs
```

Periksa apakah port **3000** sudah digunakan.

---

### Jika Port 3000 Sudah Digunakan

Jalankan aplikasi pada port lain.

```bash
PORT=3001 pm2 start npm --name nama-project -- start
```

---

### Jika Port 3000 Masih Kosong

```bash
pm2 start npm --name nama-project -- start
```

Contoh:

```bash
pm2 start npm --name youvitation -- start
```

---

### Melihat Daftar Aplikasi

```bash
pm2 list
```

---

### Menyimpan Konfigurasi PM2

Agar aplikasi otomatis berjalan kembali ketika VPS restart.

```bash
pm2 save
```

Aktifkan startup PM2.

```bash
pm2 startup
```

Ikuti perintah yang diberikan oleh PM2 hingga selesai.

---

## 🔄 Update Project di VPS

Apabila terdapat perubahan pada repository GitHub, cukup jalankan langkah berikut.

### 1. Ambil Perubahan Terbaru

```bash
git pull
```

---

### 2. Build Ulang Project

```bash
npm run build
```

---

### 3. Restart PM2

```bash
pm2 restart nama-project
```

Contoh:

```bash
pm2 restart youvitation
```

---

## ✅ Deployment Berhasil

Jika seluruh langkah di atas telah dilakukan, maka:

- ✅ Project berhasil di-clone dari GitHub.
- ✅ Dependency berhasil di-install.
- ✅ Project berhasil di-build.
- ✅ Nginx berhasil dikonfigurasi sebagai Reverse Proxy.
- ✅ PM2 berhasil menjalankan aplikasi.
- ✅ Aplikasi tetap berjalan meskipun terminal SSH ditutup.
- ✅ Project siap diakses melalui domain.

---

> 💡 **Best Practice**
>
> Untuk setiap perubahan kode, cukup jalankan:
>
> ```bash
> git pull
> npm install        # jika ada dependency baru
> npm run build
> pm2 restart nama-project
> ```
>
> Dengan langkah tersebut, aplikasi akan diperbarui tanpa perlu melakukan konfigurasi ulang pada Nginx.

---

---

# 🔥 Hosting CodeIgniter di VPS

Dokumentasi ini menjelaskan proses deployment aplikasi **CodeIgniter** (CI3 maupun CI4) ke VPS menggunakan **Nginx**, **PHP-FPM**, dan **MySQL**.

> 📌 **Catatan**
>
> Konfigurasi `root` pada Nginx berbeda antara **CodeIgniter 3** dan **CodeIgniter 4**. Pastikan menyesuaikan dengan versi yang digunakan.

---

## 🔑 1. Login ke VPS

Masuk ke VPS menggunakan SSH.

```bash
ssh username@IP_VPS
```

Contoh:

```bash
ssh ubuntu@43.134.xxx.xxx
```

---

## 📁 2. Membuat Folder Project

Masuk ke direktori web server.

```bash
cd /var/www
```

Buat folder project.

```bash
sudo mkdir -p nama_projek
```

Masuk ke folder tersebut.

```bash
cd nama_projek
```

---

## 📥 3. Clone Repository

Clone project dari GitHub.

```bash
git clone https://github.com/username/project.git .
```

---

## 🔐 4. Mengatur Permission

Berikan hak akses kepada user deployment.

```bash
sudo chown -R ubuntu:ubuntu /var/www/nama_projek
```

Khusus folder **writable**, ubah ownership menjadi `www-data`.

```bash
sudo chown -R www-data:www-data /var/www/nama_projek/writable
```

Kemudian ubah permission.

```bash
sudo chmod -R 775 /var/www/nama_projek/writable
```

---

## 📦 5. Install Dependency

Install seluruh dependency menggunakan Composer.

```bash
composer install
```

---

## ⚙️ 6. Konfigurasi File `.env`

Copy file environment apabila diperlukan.

```bash
cp env .env
```

Edit konfigurasi.

```bash
nano .env
```

Sesuaikan konfigurasi berikut:

- Database Host
- Database Name
- Database Username
- Database Password
- Base URL

Simpan perubahan menggunakan:

```
CTRL + O
ENTER
CTRL + X
```

---

## 🗄️ 7. Membuat Database

Masuk ke phpMyAdmin.

```
http://IP_VPS/phpMyAdmin
```

Buat database baru sesuai konfigurasi pada file `.env`.

---

## 🛠️ 8. Menjalankan Migration

Masuk ke folder project.

```bash
cd /var/www/nama_projek
```

Jalankan migration.

```bash
php spark migrate
```

---

## 🌐 9. Konfigurasi Nginx

Buat file konfigurasi baru.

```bash
sudo nano /etc/nginx/sites-available/nama_projek
```

Tambahkan konfigurasi berikut.

```nginx
server {

    server_name sig.jimlyassidqi.my.id www.sig.jimlyassidqi.my.id;

    # ===============================
    # Root Folder
    #
    # CodeIgniter 4 :
    # /var/www/nama_projek/public
    #
    # CodeIgniter 3 :
    # /var/www/nama_projek
    # ===============================

    root /var/www/nama_projek/public;

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {

        include snippets/fastcgi-php.conf;

        # Sesuaikan dengan versi PHP yang digunakan
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;

        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

        include fastcgi_params;

    }

    location ~ /\.ht {
        deny all;
    }

    error_log /var/log/nginx/nama_projek_error.log;
    access_log /var/log/nginx/nama_projek_access.log;

}
```

---

## 🔗 10. Mengaktifkan Konfigurasi Nginx

Buat symbolic link.

```bash
sudo ln -s /etc/nginx/sites-available/nama_projek /etc/nginx/sites-enabled/
```

Cek konfigurasi.

```bash
sudo nginx -t
```

Jika tidak terdapat error, reload Nginx.

```bash
sudo systemctl reload nginx
```

---

## 🔒 11. Memasang SSL Gratis

Install SSL menggunakan Certbot.

```bash
sudo certbot --nginx -d domainanda.com
```

Contoh:

```bash
sudo certbot --nginx -d pariwisata.jimlyassidqi.my.id
```

Ikuti seluruh proses hingga selesai.

---

## ✅ Deployment Berhasil

Apabila seluruh langkah di atas telah dilakukan, maka:

- ✅ Source code berhasil di-clone dari GitHub.
- ✅ Dependency Composer berhasil di-install.
- ✅ Database berhasil dikonfigurasi.
- ✅ Migration berhasil dijalankan.
- ✅ Nginx berhasil dikonfigurasi.
- ✅ SSL berhasil dipasang.
- ✅ Aplikasi CodeIgniter siap digunakan melalui domain.

---

## 📚 Referensi

- https://codeigniter.com/
- https://www.nginx.com/
- https://getcomposer.org/
- https://certbot.eff.org/
- https://www.php.net/

---

<div align="center">

### 🚀 Happy Deploying!

**Made with ❤️ for easier VPS deployment documentation.**

</div>

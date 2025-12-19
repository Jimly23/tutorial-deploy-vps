1. Masuk ke Folder `/var/www` dan Buat Folder Project
```
cd /var/www
sudo mkdir nama_folder
cd nama_folder
```
2. Clone Repository Project Laravel
```
sudo git clone link_github
cd nama-project
```
3. Konfigurasi File .env
```
cp .env.example .env
nano .env
```
4. Ubah konfigurasi database sesuai VPS:
```
DB_DATABASE=nama_database
DB_USERNAME=username_database
DB_PASSWORD=password_database

simpan perubahan dengan menekan ctrl+o kemudian enter, lalu untuk keluar tekan ctrl+x lalu enter
```
5. Install Dependency Laravel
```
composer install
```
6. Buat database
```
mysql -u root -p
CREATE DATABASE nama_database;
```
7. Generate Application Key
```
php artisan key:generate
```
8. Jalankan migrasi database
```
php artisan migrate
```
9. Set permission folder laravel
```
sudo chown -R www-data:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```
10. Konfigurasi nginx
```
sudo nano /etc/nginx/sites-available/nama_file

isi konfigurasi:
server {
    listen 80;
    listen [::]:80;
    server_name elearning.bitblock.my.id;
    root /var/www/elearnining.bitblock.my.id/html/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;
    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

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
11. Aktifkan konfigurasi nginx
```
sudo ln -s /etc/nginx/sites-available/nama_file /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl reload nginx
```
12. Akses aplikasi
```
http://domain-anda.com
```

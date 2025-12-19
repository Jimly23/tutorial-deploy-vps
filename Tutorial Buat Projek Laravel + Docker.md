1. Buat folder project
```
mkdir laravel-api-docker
cd laravel-api-docker
```
2. Struktur akhir
```
laravel-api-docker/
├── docker/
│   └── php/
│       └── apache.conf
├── src/                ← Laravel
├── docker-compose.yml
└── Dockerfile
```
3. Buat project laravel (api)
```
docker run --rm -v ${PWD}/src:/app composer create-project laravel/laravel . --prefer-dist --no-interaction
```
4. Buat project laravel (api)
```
docker compose up -d
docker compose exec app php artisan install:api
```
5. Dockerfile (PHP + Apache)
```
FROM php:8.2-apache

# install dependency
RUN apt-get update && apt-get install -y \
    zip unzip git curl \
    libpng-dev libonig-dev libxml2-dev \
    && docker-php-ext-install pdo_mysql mbstring bcmath gd

# enable rewrite
RUN a2enmod rewrite

# composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# apache config
COPY docker/php/apache.conf /etc/apache2/sites-available/000-default.conf

WORKDIR /var/www/html

EXPOSE 80
```
6. docker/php/apache.conf (Apache Config)
```
<VirtualHost *:80>
    DocumentRoot /var/www/html/public

    <Directory /var/www/html>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
7. docker-compose.yml (Docker Compose)
```
services:
  app:
    build: .
    container_name: laravel_app
    volumes:
      - ./src:/var/www/html
    ports:
      - "8000:80"
    environment:
      DB_CONNECTION: mysql
      DB_HOST: mysql
      DB_PORT: 3306
      DB_DATABASE: laravel
      DB_USERNAME: laravel
      DB_PASSWORD: secret
    depends_on:
      - mysql

  mysql:
    image: mysql:8
    container_name: laravel_mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: laravel
      MYSQL_USER: laravel
      MYSQL_PASSWORD: secret
    volumes:
      - mysql_data:/var/lib/mysql
    ports:
      - "3306:3306"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: laravel_phpmyadmin
    ports:
      - "8001:80"
    environment:
      PMA_HOST: mysql
      MYSQL_ROOT_PASSWORD: root
    depends_on:
      - mysql

volumes:
  mysql_data:
```
8. src/.env (Set .env Laravel)
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```
9. Build & Run Docker
```
docker compose build
docker compose up -d
```
10. Generate Key & Migrate
```
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
```

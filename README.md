 apt update

apt install nginx
 mkdir projek
 cd projek/
git clone https://github.com/Fadil08/farmasi
#pindahkan code ke file /var/www/
mv farmasi /var/www


nano /etc/nginx/sites-available/farmasi.conf
#isi File farmasi.conf
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
#    root /var/www/test;
    root /var/www/farmasi/public;

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

    location ~ \.php$ {
  #      try_files =$uri = 404;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
#membuat symbolic link
ln -s /etc/nginx/sites-available/farmasi.conf /etc/nginx/sites-enabled/
#Restart Service Nginx
Systemctl restart nginx
#install PHP dan konfigurasi
install php and its dependencies
sudo apt install php8.1-fpm php8.1 php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip php8.1-intl php8.1-bcmath unzip -y
php -v
sudo nano /etc/php/8.1/fpm/php.ini
cgi.fix_pathinfo=1
sudo nano /etc/php/8.1/fpm/pool.d/www.conf
#isi www.conf
user = www-data
group = www-data
listen.owner = www-data
listen.group = www-data
listen.allowed_clients = 127.0.0.1

#Install composercurl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
composer --version

#Install mysql-server
sudo apt install mysql-server

mysql

alter user 'root'@'localhost' identified with mysql_native_password by 'NewPassword';

exit

sudo mysql_secure_installation

mysql -u root -p

CREATE DATABASE laravel_db;

CREATE USER 'laravel_user'@'localhost' IDENTIFIED BY 'P@ssWord';

GRANT ALL PRIVILEGES ON laravel_db.* TO 'laravel_user'@'localhost';

FLUSH PRIVILEGES; SHOW DATABASES;

select user,url from users;

exit

cp -r farmasi /var/www/
cd /var/www/
cd farmasi/
composer update
ls
nano .env
php artisan migrate:fresh
php artisan migrate
php artisan db:seed
nano /etc/nginx/sites-available/farmasi.conf
nginx -t
systemctl restart nginx
sudo chown -R www-data:www-data storage
chmow 777 public/
chmod 777 public/
sudo chown -R www-data:www-data public/




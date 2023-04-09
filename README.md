
# Ekspor Nurhadi

This a development repository for Ekspor Nurhadi Project<br>
Developed by Bagisto using Laravel framework<br>
Feel free to modify, before we go to real development repo.

------------
<br>

## DEVELOPMENT Setup
We suggest you are using docker container to develop this repository, provided in https://github.com/fbhikmawan/ekspor-docker<br>

(-) Clone the Docker Repository
```
git clone git@github.com:fbhikmawan/ekspor-docker.git
```
(-) Enter the repository and change the branch to develop
```
cd ekspor-docker
git checkout develop
```
(-) Modify the PORT ASSIGNMENTS in ".env" file according to your needs or if you encounter port conflict.

(-) Run the Docker
```
docker-container up
```
<br>
After all the docker services are running successfully. 

(1) Clone this ekspor-nurhadi repository to folder /workspace
```
# /workspace
git clone git@github.com:fbhikmawan/ekspor-nurhadi.git
```
(2) Rename the folder to "bagisto"
```
# /workspace
mv ekspor-nurhadi bagisto
```
(3) Change the branch to develop
```
# /workspace/bagisto
git checkout develop
```
(4) Copy the .env.example file to .env file. Edit as your needs
```
cp .env.example .env
```
Make sure about:

* Application Properties
* Database Connection
* Database Credential
* Database Name

(5) Install all the PHP vendor dependencies
```
composer install
```
(6) Generate the Application Key
```
php artisan key:generate
```
(7) Migrate the application database structures
```
php artisan migrate
```
(8) Seeds the database with existing data
```
php artisan db:seed
```
(9) Publish the vendor 
```
php artisan vendor:publish
```
(10) Link the storage folder 
```
php artisan storage:link
```
(11) Dumping the vendor dependencies
```
# /workspace
composer dump-autoload -d bagisto/
```
(DONE) You can access the front at `http://localhost:8001`. the admin page at `http://localhost:8001/admin` with credential username `admin@example.com` and password `admin123`


------------
<br>

## DEPLOYMENT Setup

Prepare your server:

(-) Make sure your server meets the minimum requirements.

<strong>Server configuration</strong>
* SERVER: Apache 2 or NGINX.
* RAM: 4GB or higher.
* Node: 8.11.3 LTS or higher.
* PHP: 7.4 or higher.
* Composer: 1.6.5 or higher.

<strong>PHP Extensions</strong><br>
Make sure the following extensions are installed and enabled. You can check using the phpinfo() page or the php -m command.
* php-intl extension
* php-gd extension

(-) Clone the repository to your desired folder. Lets say on folder "`\home\username`". Then, the repository full path will be "`\home\username\ekspor-nurhadi`".

Begin to deploy the app:

(1) Change the branch to develop
```
git checkout develop
```
(2) Copy the .env.example file to .env file. Edit as your needs
```
cp .env.example .env
```
Make sure about:

* Application Properties
* Database Connection
* Database Credential
* Database Name

(3) Install all the PHP vendor dependencies
```
composer install
```
(4) Generate the Application Key
```
php artisan key:generate
```
(5) Migrate the application database structures
```
php artisan migrate
```
(6) Seeds the database with existing data
```
php artisan db:seed
```
(7) Publish the vendor 
```
php artisan vendor:publish
```
(8) Link the storage folder 
```
php artisan storage:link
```
(9) Dumping the vendor dependencies
```
# /workspace
composer dump-autoload -d bagisto/
```
(10) Update the folder permission for Laravel's logging activities. Webserver user and group depend on your webserver and your OS. To figure out what's your web server user and group use the following commands.

For NGINX use:
```
ps aux|grep nginx|grep -v grep
```
Then update the owner and permission.<br>
The string 'www-data' below is taken from script above. 
```
sudo chown -R $USER:www-data storage
sudo chown -R $USER:www-data bootstrap/cache
```
```
chmod -R 775 storage
chmod -R 775 bootstrap/cache
```
(11) Move the repository to `/srv/` folder
```
mv /home/username/ekspor-nurhadi /srv/
```
(12) Create the nginx configuration file named "`example.com`" to the folder "`/etc/nginx/sites-available`"
```
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /srv/ekspor-nurhadi/public;
 
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
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

(13) Create a link those config file to NGINX site-enable:
```
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

(14) Check the overall NGINX configuration files:
```
sudo nginx -t
```
Ensure the return status is OK.

(15) Restart the NGINX service:
```
sudo nginx -s reload
```
(DONE) You can access the front at `http://example.com`. the admin page at `http://example.com/admin` with credential username `admin@example.com` and password `admin123`
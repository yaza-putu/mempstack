# mempstack
MEMP Stack. MacOSX + Nginx + MySQL + PHP

# 1. install brew
- install => https://brew.sh/

# 2. install nginx
```bash
brew install nginx
```
# 3. Install php include php-fpm
```bash
brew install php
// or spesific version
brew install php@7.4
```
# 4. Install Mysql
```bash
brew install mysql
```
# Configuration after instalation on the top
## first setup mysql
- start service mysql
```bash
brew services start mysql
```
- configuration mysql
```bash
mysql_secure_installation
```
- follow the instructions until finish
- start mysql serve
```bash
mysql.server start
```
- login to mysql from terminal with password your first setup
```bash
mysql -u root -p
```
- Change the authentication method for root as mysql_native_password to support login in phpmyadmin
```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yourpassword';
```
- Reload grant tables
```bash
FLUSH PRIVILEGES;
```
- finish and exit
```bash
exit
```

# Config PHP
- export path php
```bash
sudo nano ~/.zshrc
// or
sudo nano ~/.bash_profile
```
- add
```conf
export PATH="/usr/local/opt/php@7.3/bin:$PATH"
```
note: php@7.3 same with installed php in your mac
- check your php and php-pfm
```bash
php -v
php-fpm -v
```

# config nginx
- create config nginx for your project with ext *.conf, ex: phpmyadmin.conf
```conf
server {
    listen 9090;
    index index.php;
    server_name phpmyadmin.local;
    root /root/for/your/project;

    access_log  /usr/local/etc/nginx/logs/phpmyadmin.access.log;
    error_log /usr/local/etc/nginx/logs/phpmyadmin.error.log;

    client_max_body_size 100m;
    client_body_buffer_size 512k;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param SCRIPT_NAME $fastcgi_script_name;

        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        include /usr/local/etc/nginx/fastcgi.conf;
    }
}
```
- put on directory /usr/local/etc/nginx/servers
note : show hidden file with keyboard shortcuts cmd + shift + .
- make dir for log nginx
```bash
mkdir /usr/local/etc/nginx/logs
```
- cek your config health with
```bash
nginx -t
```
## basic opration nginx
- start
```bash
sudo nginx
```
- stop
```bash
sudo nginx -s stop
```
- reload
```bash
sudo nginx -s reload
```

## basic opration mysql
- start
```bash
mysql.server start
```
- stop
```bash
mysql.server stop
```
note: after change config file .conf nginx must be check config with nginx-t and reload nginx with sudo nginx -s reload

# Finish
cek your with localhost:9090 (config port on file *.conf)

note : first on your laptop must be run service nginx and mysql





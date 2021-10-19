# mempstack
MEMP Stack. MacOSX + Nginx + MySQL + PHP

# 1. install brew
- install => https://brew.sh/

# 2. install nginx
```bash
brew install nginx
```
## config nginx
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





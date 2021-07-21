# Lychee

- Very nice and polished UI
- Supports multiple users
- Supports U2F
- Can import from local server path, URL or Dropbox
- Supports creating and sharing nested albums!

<br>

- [Github repo](https://github.com/LycheeOrg/Lychee)
- [Homepage](https://lycheeorg.github.io/)
- [Docs](https://lycheeorg.github.io/docs/docker.html)


## nginx.conf
This file is required to increase the upload size (from the default 20MB).
Original can be found here: [default.conf](https://github.com/LycheeOrg/Lychee-Docker/blob/master/default.conf).
This will increase the size to 1000MB (`upload_max_filesize` and `post_max_size`):

```nginx
user www-data;
worker_processes auto;
daemon off;
error_log /var/log/nginx/error.log;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    keepalive_timeout  65;
    # By default, if the processing of images takes more than 60s,
    # a 504 Gateway timeout occurs, so we increase the timeout here
    # to allow procesing of large images or when multiple images are
    # being processed at the same time. We set max_execution_time
    # below to the same value.
    fastcgi_read_timeout 3600;
    # We also set the send timeout since this can otherwise also cause
    # issues with slow connections
    fastcgi_send_timeout 3600;
    gzip  on;
    server {
        root /var/www/html/Lychee/public;
        listen       80;
        server_name  localhost;
        client_max_body_size 100M;
        # serve static files directly
        location ~* \.(jpg|jpeg|gif|css|png|js|ico|html)$ {
            access_log off;
            expires max;
            log_not_found off;
        }
        # removes trailing slashes (prevents SEO duplicate content issues)
        if (!-d $request_filename)
        {
            rewrite ^/(.+)/$ /$1 permanent;
        }
        # If the request is not for a valid file (image, js, css, etc.), send to bootstrap
        if (!-e $request_filename)
        {
            rewrite ^/(.*)$ /index.php?/$1 last;
            break;
        }
        location / {
            index  index.php
            try_files $uri $uri/ /index.php?$query_string;
        }
        # Serve /index.php through PHP
        location = /index.php {
            fastcgi_split_path_info ^(.+?\.php)(/.*)$;
            try_files $uri $document_root$fastcgi_script_name =404;
            # Mitigate https://httpoxy.org/ vulnerabilities
            fastcgi_param HTTP_PROXY "";
            fastcgi_pass unix:/run/php/php7.4-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param PHP_VALUE "post_max_size=1000M
                max_execution_time=3600
                upload_max_filesize=1000M
                memory_limit=1024M";
            fastcgi_param PATH /usr/local/bin:/usr/bin:/bin;
            include fastcgi_params;
        }
        # Deny access to other .php files, rather than exposing their contents
        location ~ [^/]\.php(/|$) {
            return 403;
        }
    }
}
```


## docker-compose.yml
```yml
---
version: '3'
services:
  lychee_db:
    container_name: lychee_db
    image: mariadb:10
    restart: unless-stopped
    environment:
      - MYSQL_ROOT_PASSWORD=lychee123
      - MYSQL_DATABASE=lychee
      - MYSQL_USER=lychee
      - MYSQL_PASSWORD=lychee123
    expose:
      - 3306
    volumes:
      - ./db:/var/lib/mysql

  lychee:
    image: lycheeorg/lychee
    container_name: lychee
    restart: unless-stopped
    depends_on:
      - lychee_db
    ports:
      - 3123:80
    volumes:
      - ./lychee/conf:/conf
      - ./lychee/uploads:/uploads
      - ./lychee/sym:/sym
      - ./nginx.conf:/etc/nginx/nginx.conf
    environment:
      - PUID=1000
      - PGID=1000
      - PHP_TZ=Europe/Dublin
      - DB_CONNECTION=mysql
      - DB_HOST=lychee_db
      - DB_PORT=3306
      - DB_DATABASE=lychee
      - DB_USERNAME=lychee
      - DB_PASSWORD=lychee123
      - STARTUP_DELAY=0

      #- APP_NAME=Laravel
      #- APP_ENV=local
      #- APP_URL=http://localhost
      #- CACHE_DRIVER=file
      #- SESSION_DRIVER=file
      #- SESSION_LIFETIME=120
      #- QUEUE_DRIVER=sync
      #- REDIS_HOST=127.0.0.1
      #- REDIS_PASSWORD=null
      #- REDIS_PORT=6379
      #- MAIL_DRIVER=smtp
      #- MAIL_HOST=smtp.mailtrap.io
      #- MAIL_PORT=2525
      #- MAIL_USERNAME=null
      #- MAIL_PASSWORD=null
      #- MAIL_ENCRYPTION=null
```

**Note:** when you open the url first time (e.g. http://localhost:3123) you will see an installer. There should be no need to change anything there, and just clicking "Next" should work.

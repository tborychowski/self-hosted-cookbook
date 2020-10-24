# NextCloud
- Mobile apps are just for files, not other apps (e.g. tasks) installed in NextCloud.

<br>

- [Homepage](https://nextcloud.com/)
- [Github repo](https://github.com/nextcloud)
- [DockerHub repo](https://hub.docker.com/_/nextcloud/)
- [Breeze dark theme](https://github.com/mwalbeck/nextcloud-breeze-dark) - some stuff doesn't work
- [Docs](https://docs.nextcloud.com/server/20/admin_manual/)
- [Demo](https://try.nextcloud.com/)


## docker-compose.yml
```yml
version: '2'
services:
  app:
    image: nextcloud:latest
    container_name: nextcloud
    restart: unless-stopped
    links:
      - db
      - redis
    ports:
      - 3100:80
    environment:
      - MYSQL_PASSWORD=nextcloud
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db
      - TRUSTED_PROXIES=               # e.g. HOST IP
      - OVERWRITEPROTOCOL=https
      - OVERWRITEHOST=nextcloud.domain.com
      - REDIS_HOST=redis
      - REDIS_HOST_PASSWORD=nextcloud  # must be set, because of a bug
    volumes:
      - ./nextcloud:/var/www/html
      - ./data:/var/www/html/data

  cron:
    image: nextcloud:latest
    restart: unless-stopped
    volumes:
      - ./nextcloud:/var/www/html
    entrypoint: /cron.sh
    depends_on:
      - db
      - redis

  db:
    image: mariadb
    container_name: nextcloud-db
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    restart: unless-stopped
    volumes:
      - ./db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=nextcloud
      - MYSQL_PASSWORD=nextcloud
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  redis:
    image: redis:alpine
    container_name: nextcloud-redis
    restart: unless-stopped
    command: redis-server --requirepass nextcloud
    environment:
      - TZ=Europe/Dublin
    volumes:
      - ./redis:/data
    expose:
      - 6379
```

## Tips & Tricks

### Generate password/secret key
```sh
openssl rand -base64 32
```

### Enable HEIC & MOV previews

1. Make sure that Preview Generator app is installed
2. Add this section to the `config.php`
  ```php
    'enable_previews' => true,
    'enabledPreviewProviders' => [
      'OC\Preview\PNG',
      'OC\Preview\JPEG',
      'OC\Preview\GIF',
      'OC\Preview\HEIC',
      'OC\Preview\BMP',
      'OC\Preview\XBitmap',
      'OC\Preview\MP3',
      'OC\Preview\TXT',
      'OC\Preview\MarkDown',
      'OC\Preview\OpenDocument',
      'OC\Preview\Krita',
      'OC\Preview\Movie',
      'OC\Preview\MKV',
      'OC\Preview\MP4',
      'OC\Preview\AVI',
      'OC\Preview\MSOffice2003',
      'OC\Preview\MSOffice2007',
      'OC\Preview\MSOfficeDoc',
      'OC\Preview\PDF',
      'OC\Preview\SVG',
    ],
  ```

3. If you don't see the previews, try running this command:
```sh
docker-compose exec -u www-data app bash -c ."/occ preview:generate-all -vvv"
```

4. For video previews you need to install ffmpeg, like so:
```sh
docker-compose exec app bash -c "apt update && apt upgrade -y && apt install -y ffmpeg"
# optionally add imagemagick and ghostscript
```
5. This will not persist so it must be run every time the container restarts...
6. Alternative is to manually build docker image :-|

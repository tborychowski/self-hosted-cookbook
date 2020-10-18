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

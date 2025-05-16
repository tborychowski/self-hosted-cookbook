# PhotoView

Photo gallery for self-hosted personal servers.

<br>

- [Github repo](https://github.com/viktorstrate/photoview)


## docker-compose.yml
```yml
services:
  db:
    image: mariadb
    restart: unless-stopped
    environment:
      - MYSQL_DATABASE=photoview
      - MYSQL_USER=photoview
      - MYSQL_PASSWORD=photo-secret
      - MYSQL_RANDOM_ROOT_PASSWORD=1
    volumes:
      - ./db:/var/lib/mysql

  photoview:
    image: viktorstrate/photoview:latest
    restart: unless-stopped
    ports:
      - "3090:80"
    depends_on:
      - db
    environment:
      - PHOTOVIEW_DATABASE_DRIVER=mysql
      - PHOTOVIEW_MYSQL_URL=photoview:photo-secret@tcp(db)/photoview
      - PHOTOVIEW_LISTEN_IP=photoview
      - PHOTOVIEW_LISTEN_PORT=80
      - PHOTOVIEW_MEDIA_CACHE=/app/cache
    volumes:
      - ./cache:/app/cache
      - ./photos:/photos:ro
```

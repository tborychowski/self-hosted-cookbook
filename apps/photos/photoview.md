# PhotoView

Photo gallery for self-hosted personal servers.

<br>

- [Github repo](https://github.com/viktorstrate/photoview)


## docker-compose.yml
```yml
version: "3"
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
      - MYSQL_URL=photoview:photo-secret@tcp(db)/photoview
      - API_LISTEN_IP=photoview
      - API_LISTEN_PORT=80
      - PHOTO_CACHE=/app/cache
      - PUBLIC_ENDPOINT=http://192.168.1.10:3090/
    volumes:
      - ./cache:/app/cache
      - ./photos:/photos:ro
```

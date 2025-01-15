# Bookstack

- Has some structure: Shelf > Book > Chapter > Page
- Looks modern, but the UX is not great and not very configurable


<br>

- [Homepage](https://www.bookstackapp.com/)
- [Github repo](https://github.com/BookStackApp/BookStack)
- [DockerHub repo](https://hub.docker.com/r/linuxserver/bookstack)


## docker-compose.yml
```yml
---
services:
  bookstack:
    image: linuxserver/bookstack
    container_name: bookstack
    environment:
      - PUID=1000
      - PGID=1000
      - DB_HOST=db
      - DB_USER=bookstack
      - DB_PASS=bookstack
      - DB_DATABASE=bookstack
    volumes:
      - ./data:/config
    ports:
      - 6875:80
    restart: unless-stopped
    depends_on:
      - db
  db:
    image: linuxserver/mariadb
    container_name: bookstack-db
    environment:
      - PUID=1000
      - PGID=1000
      - MYSQL_ROOT_PASSWORD=bookstack
      - TZ=Europe/Dublin
      - MYSQL_DATABASE=bookstackapp
      - MYSQL_USER=bookstack
      - MYSQL_PASSWORD=bookstack
    volumes:
      - ./data:/config
    restart: unless-stopped
```


## Tips & Tricks
- The app takes a while to start up first time, so be patient!
- Login with:  `admin@admin.com`:`password`

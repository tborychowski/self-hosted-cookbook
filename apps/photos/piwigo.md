# Piwigo

- Has mobile app
- Some video support
- Quite complex to manage, but also pretty powerful

<br>

- [Homepage](http://piwigo.org/)
- [Github repo](https://github.com/Piwigo)
- [DockerHub repo](https://hub.docker.com/r/linuxserver/piwigo)


## docker-compose.yml
```yml
---
version: "2"
services:
  piwigo:
    image: linuxserver/piwigo
    container_name: piwigo
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
    depends_on:
      - db
    ports:
      - 3060:80
    volumes:
      - ./config:/config

  db:
	image: mariadb
    restart: unless-stopped
    environment:
        - MYSQL_ROOT_PASSWORD=piwigo
        - MYSQL_DATABASE=piwigo
```

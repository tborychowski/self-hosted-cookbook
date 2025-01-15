# Jackett

API Support for your favorite torrent trackers.

<br>

- [Github repo](https://github.com/Jackett/Jackett)


## docker-compose.yml
```yml
---
services:
  jackett:
    image: linuxserver/jackett
    container_name: jackett
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
      - RUN_OPTS=run options here #optional
    ports:
      - 9117:9117
    volumes:
      - ./data:/config
      - ./downloads:/downloads
```

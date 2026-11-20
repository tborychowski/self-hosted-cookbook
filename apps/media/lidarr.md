# Lidarr

Music manager.

<br>

- [Homepage](https://lidarr.audio/)
- [Github repo](https://github.com/Lidarr/Lidarr)


## docker-compose.yml
```yml
services:
  lidarr:
    image: lscr.io/linuxserver/lidarr:latest
    container_name: lidarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
    volumes:
      - ./config:/config
      - ./music:/music
      - ./downloads:/downloads
    ports:
      - 8686:8686
```

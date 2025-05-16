# Deluge
- weird
- ugly
- can't disable password

<br>

- [Homepage](https://deluge-torrent.org/)
- [Git repo](https://git.deluge-torrent.org/deluge)
- [DockerHub repo](https://hub.docker.com/r/linuxserver/deluge)


## docker-compose.yml
```yml
services:
  deluge:
    image: linuxserver/deluge
    container_name: deluge
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
#      - UMASK_SET=022 #optional
#      - DELUGE_LOGLEVEL=error #optional
    ports:
      - 3124:8112
    volumes:
      - ./config:/config
      - ./downloads:/downloads
```

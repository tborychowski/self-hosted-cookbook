# Radarr

Movie manager.

<br>

- [Homepage](https://radarr.video/)
- [Github repo](https://github.com/Radarr/Radarr)


## docker-compose.yml
```yml
---
services:
  radarr:
    image: linuxserver/radarr
    container_name: radarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
      - UMASK_SET=022 #optional
    ports:
      - 7878:7878
    volumes:
      - ./config:/config
      - /mnt/video:/video
```


## Tips & Tricks
See [Sonarr](sonarr.md).

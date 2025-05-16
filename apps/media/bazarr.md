# Bazarr

Bazarr is a companion application to Sonarr and Radarr. It manages and downloads subtitles based on your requirements.

<br>

- [Homepage](https://www.bazarr.media/)
- [Github repo](https://github.com/morpheus65535/bazarr)


## docker-compose.yml
```yml
services:
  bazarr:
    image: linuxserver/bazarr
    container_name: bazarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
      - UMASK_SET=022 #optional
    ports:
      - 6767:6767
    volumes:
      - ./config:/config
      - /mnt/video:/video
```

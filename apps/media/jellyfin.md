# Jellyfin
Audio, Video & Photo manager.<br>

- Allows you to view all the media from your local server on almost any device, anywhere.

<br>

- [Homepage](https://jellyfin.org/)

okdocker run -d -v /srv/jellyfin/config:/config -v /srv/jellyfin/cache:/cache -v /media:/media --net=host jellyfin/jellyfin:latest


## docker-compose.yml
```yml
---
services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    network_mode: host
    # user: uid:gid
    restart: unless-stopped
    # Optional - alternative address used for autodiscovery
    # environment:
    #   - JELLYFIN_PublishedServerUrl=http://example.com
    ports:
      - 8096:8096
    volumes:
      - ./config:/config
      - ./cache:/cache
      - ./media:/media
```

Server should be available at: `<serverIP>:8096`

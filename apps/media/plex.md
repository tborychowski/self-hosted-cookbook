# Plex
Audio, Video & Photo manager.<br>

- Allows you to view all the media from your local server on any device, anywhere.
- Allows you to add any other paid streaming service and search through them.
- Requires Plex account for authentication.
- Requires Plex subscription for some extra features.

<br>

- [Homepage](https://www.plex.tv/)


## docker-compose.yml
```yml
---
services:
  plex:
    image: ghcr.io/linuxserver/plex
    container_name: plex
    network_mode: host
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - VERSION=docker
    devices:
      - /dev/dri:/dev/dri
    volumes:
      - ./config:/config
      - ./tvshows:/Shows
      - ./movies:/Movies
      - ./music:/Music
```

Server should be available at: `<serverIP>:32400`

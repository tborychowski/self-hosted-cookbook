# Homarr

Customizable browser's home page to interact with your homeserver's Docker containers (e.g. Sonarr/Radarr).

<br>

- [Github repo](https://github.com/ajnart/homarr)


## docker-compose.yml
```yml
---
services:
  homarr:
    image: ghcr.io/ajnart/homarr:latest
    container_name: homarr
    restart: unless-stopped
    volumes:
      - ./homarr/configs:/app/data/configs
      - ./homarr/icons:/app/public/icons
    ports:
      - 7575:7575
```

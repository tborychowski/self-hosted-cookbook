# Navidrome

Music streaming service.

<br>

- [Homepage](https://www.navidrome.org/)
- [Github repo](https://github.com/deluan/navidrome/)
- [Demo](https://www.navidrome.org/demo/)

![screenshot](navidrome.png)


## docker-compose.yml
```yml
---
version: "3"
services:
  navidrome:
    image: deluan/navidrome:latest
    restart: unless-stopped
    ports:
      - "4533:4533"
    environment:
      # All options with their default values:
      ND_SCANINTERVAL: 1m
      ND_LOGLEVEL: info
      ND_SESSIONTIMEOUT: 30m
      ND_BASEURL: ""
    volumes:
      - ./data:/data
      - ./music/:/music:ro
```

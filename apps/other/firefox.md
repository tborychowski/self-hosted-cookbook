# Firefox
It's a browser inside a browser!

- Very useful when you need to check a site that is blocked by your provider (work/school) (assuming that the firefox instance you host is not blocked).
- a bit slow, but it works!

<br>

- [Github repo](https://github.com/linuxserver/docker-firefox)


## docker-compose.yml
```yml
---
services:
  firefox:
    image: lscr.io/linuxserver/firefox:latest
    container_name: firefox
    shm_size: "1gb"
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
    ports:
      - 3123:3000
    volumes:
      - ./config:/config
```

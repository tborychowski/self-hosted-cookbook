# Duplicati

- no notifications!
- ugly as hell
- confusing to set-up

<br>

- [Github repo](https://github.com/duplicati/duplicati)
- [Docker Hub](https://hub.docker.com/r/linuxserver/duplicati)
- [Homepage](https://www.duplicati.com/)


## docker-compose.yml
```yml
---
version: '3.3'
services:
  duplicati:
    image: linuxserver/duplicati
    container_name: duplicati
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
#      - CLI_ARGS= #optional
    ports:
      - 8200:8200
    volumes:
      - ./config:/config

      - /mnt/backups:/backups/backups              # backup target
      - ~:/backups/home:ro                         # backup source - home
	  - /mnt/docker:/backups/docker:ro             # backup source - dockers
```

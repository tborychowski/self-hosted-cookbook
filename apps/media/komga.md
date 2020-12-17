# Komga

Probably the best self-hosted comic books reader.

<br>

- [Homepage](https://komga.org/)
- [Github repo](https://github.com/gotson/komga)
- [DockerHub repo](https://hub.docker.com/r/gotson/komga)
- [Docs](https://komga.org/guides/)


## docker-compose.yml
```yml
---
version: '3.3'
services:
    komga:
        image: gotson/komga
        container_name: komga
        restart: unless-stopped
        user: "1000:1000"
        environment:
            - KOMGA_LIBRARIES_SCAN_DIRECTORY_EXCLUSIONS=#recycle,@eaDir
        ports:
            - 3020:8080
        volumes:
            - ./data:/config
            - ./books:/books
            - /etc/timezone:/etc/timezone:ro
```


## Tips & Tricks

### First steps
- After the first run - admin account gets created.
- You need to check the logs (`docker-compose logs komga` or in `./data` folder)
- Comics should be added to the `./books` folder

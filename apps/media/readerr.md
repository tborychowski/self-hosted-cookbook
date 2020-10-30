# Readerr

Ebook & comics manager.

<br>

- [Homepage](https://readarr.com/)
- [Github repo](https://github.com/Readarr/Readarr)


## docker-compose.yml
```yml
version: "3.5"
services:
  readarr:
    image: hotio/readarr:nightly
    container_name: readarr
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=Europe/Dublin
      - UMASK=002
    volumes:
      - ./config:/config
    ports:
      - 8787:8787
    restart: unless-stopped
```

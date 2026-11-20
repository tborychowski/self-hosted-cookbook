# Readerr

Ebook & comics manager.

<br>

- [Homepage](https://readarr.com/)
- [Github repo](https://github.com/Readarr/Readarr)


## docker-compose.yml
```yml
services:
  readarr:
    image: hotio/readarr:nightly
    container_name: readarr
    restart: unless-stopped
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=Europe/Dublin
    volumes:
      - ./config:/config
    ports:
      - 8787:8787
```

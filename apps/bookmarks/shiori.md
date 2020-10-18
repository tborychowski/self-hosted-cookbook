# Shiori
- [Github repo](https://github.com/go-shiori/shiori)
- [Docs](https://github.com/go-shiori/shiori/wiki)

![Screenshot](shiori.png)


## docker-compose.yml
```yml
---
version: '3'
services:
  shiori:
    image: radhifadlillah/shiori
    container_name: shiori
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
    ports:
      - "3010:8080"
    volumes:
      - ./data:/srv/shiori
```

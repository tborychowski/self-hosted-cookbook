# Bluespice free

- Terrible UX, unintuitive and just ugly
- Even with the domain set up - it sometimes redirect to `localhost`

<br>

- [Homepage](https://bluespice.com/products/bluespice-free/)
- [DockerHub repo](https://hub.docker.com/r/bluespice/bluespice-free)


## docker-compose.yml
```yml
---
version: '3.3'
services:
  bluespice:
    image: bluespice/bluespice-free
    container_name: bluespice
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
      - bs_lang=en
      - bs_url=https://bluespice.example.com
    ports:
      - 3123:80
    volumes:
      - ./data:/data
```

### Login with
- username: WikiSysop
- password: PleaseChangeMe

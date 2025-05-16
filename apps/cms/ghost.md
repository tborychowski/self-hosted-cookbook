# Ghost
Flat file cms/blogging platform.

<br>

- [Github repo](https://github.com/TryGhost/Ghost)
- [Docs](https://ghost.org/docs/install/docker/)


## docker-compose.yml
```yml
services:
  ghost:
    image: ghost:4-alpine
    container_name: ghost
    restart: unless-stopped
    ports:
      - 2368:2368
    environment:
      # https://ghost.org/docs/config/#configuration-options
      url: http://<serverIP>:2368
    volumes:
      - ./data:/var/lib/ghost/content
```

After the startup, open `http://<serverIP>:2368/ghost` to finish setup.

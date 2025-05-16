# AudioBookshelf
Self-hosted Audiobook Server.
- It takes a while to adjust the folder structure and file naming for the scanner to pick it up correctly (and then manually fixing them in the UI)
- Looks good and works nicely in a desktop browser
- Unfortunately it doesn't work on mobile


<br>

- [Homepage](https://www.audiobookshelf.org)
- [Github repo](https://github.com/advplyr/audiobookshelf)


## docker-compose.yml
```yml
services:
  audiobookshelf:
    image: advplyr/audiobookshelf
    container_name: audiobookshelf
    restart: unless-stopped
    ports:
      - "1337:80"
    volumes:
      - ./config:/config
      - ./metadata:/metadata
      - ./audiobooks:/audiobooks
```

Default username is **root** with no password.

# PhotoStructure

*"Your new home for all your photos & videos"*

- Very good looking
- Good support for photo and video files
- Still in early development, so limited features (no accounts, sharing, etc.)


<br>

- [Homepage](https://photostructure.com/)
- [Docs for Docker](https://photostructure.com/server/photostructure-for-docker-compose/)
- [Desktop version](https://photostructure.com/install/)

## docker-compose.yml
```yml
---
version: "3.7"
services:
  photostructure:
    image: photostructure/server
    restart: unless-stopped
    stop_grace_period: 2m
    ports:
      - 1787:1787/tcp
    volumes:
      - ./library:/ps/library
      - ./cache:/ps/tmp
      - ./config/:/ps/config
      - ./logs:/ps/logs
      -  /mnt/photos:/photos
```

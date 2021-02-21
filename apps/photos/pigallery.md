# Pigallery2

- Decent design & UX
- Good support for photo and video files
- Lots of additional features (map, faces, sharing, etc.)

<br>

- [Homepage](http://bpatrik.github.io/pigallery2/)
- [Github repo](https://github.com/bpatrik/pigallery2)
- [Demo](https://pigallery2.herokuapp.com/gallery/)


## docker-compose.yml
```yml
---
version: '3'
services:
  pigallery2:
    image: bpatrik/pigallery2:latest
    container_name: pigallery2
    environment:
      - NODE_ENV=production
    volumes:
      - ./db:/app/data/db
      - ./data/config:/app/data/config
      - ./data/tmp:/app/data/tmp
      - ./data/images:/app/data/images
    ports:
      - 3000:80
    restart: unless-stopped
```

And login with *admin*:*admin*

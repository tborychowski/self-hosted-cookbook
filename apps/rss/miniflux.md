# Miniflux

The best RSS reader that I've tested. Simple, extremely fast and minimalistic.

<br>

- [Homepage](https://miniflux.app/)
- [Github repo](https://github.com/miniflux/v2)
- [Docs](https://miniflux.app/docs/index.html)


## docker-compose.yml
```yml
version: '3'
services:
  miniflux:
    image: miniflux/miniflux:nightly
    ports:
      - "5010:8080"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgres://miniflux:secret@db/miniflux?sslmode=disable
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME=admin
      - ADMIN_PASSWORD=test123
  db:
    image: postgres:latest
    environment:
      - POSTGRES_USER=miniflux
      - POSTGRES_PASSWORD=secret
    volumes:
      - ./db:/var/lib/postgresql/data
```


## Tips & Tricks
- [Rules](https://miniflux.app/docs/rules.html) can be set for every feed source:

### Filter Rules - block rules
exclude articles containing a word (case insensitive), e.g.
```
(?i)windows,(?i)miniflux
```

### Rewrite rules
```
add_dynamic_image,add_image_title,add_youtube_video
```

### Scraper rules
This is just a css selector for the main article DOM element, e.g.:
```css
#phContent_divMetaBody>p, #phContent_divMetaBody>.content-image-wrapper
```

# Miniflux-filter

Filter for miniflux - "mark as read" all unwanted articles.
<br>
I created that before miniflux added some basic filtering.
<br>
The difference from the built-in filtering is that the built-in filtering filters articles out BEFORE adding them to the DB, whereas this just marks them as read, so you can still go to "All" and see them if you wish.
<br>
<br>
The new version adds a UI for managing filters. The UI "borrows" the css & javascript from Miniflux, so the look & feel is (almost) exactly the same as the main app!

<br>

- [Github repo](https://github.com/tborychowski/miniflux-filter)


## docker-compose.yml
```yml
---
version: '3'
services:
    miniflux-filter:
    image: tborychowski/miniflux-filter:latest
    container_name: miniflux-filter
    restart: unless-stopped
    environment:
        - TZ=Europe/Dublin
        # if not present - there will be no auth
        # - ADMIN_PASSWORD=admin1
        # ERROR, WARNING, INFO, DEBUG
        - LOG_LEVEL=INFO
    ports:
        - "5020:80"
    volumes:
        - ./data:/var/www/html/store
```

# Wiki.js

- nice looking UI, with dark mode
- nice search
- configurable & lots of modules/plugins (albeit most of them are useless)
- limited number of code highlighting (no yaml or bash...)
- the navigation logic is extremely weird... Impossible to easily build a tree-like structure (with indefinite nesting level)
- not nearly as feature rich as confluence

<br>

- [Homepage](https://js.wiki)
- [Github repo](https://github.com/Requarks/wiki)
- [Docs](https://docs.requarks.io/install/docker)


## docker-compose.yml
```yml
version: "3"
services:
  db:
    image: postgres:11-alpine
    container_name: wiki.js-db
    logging:
      driver: "none"
    restart: unless-stopped
    environment:
      POSTGRES_DB: wiki
      POSTGRES_PASSWORD: wikijsrocks
      POSTGRES_USER: wikijs
    volumes:
      - ./db:/var/lib/postgresql/data

  wiki:
    image: requarks/wiki:2
    container_name: wiki.js
    depends_on:
      - db
    environment:
      DB_TYPE: postgres
      DB_HOST: db
      DB_PORT: 5432
      DB_USER: wikijs
      DB_PASS: wikijsrocks
      DB_NAME: wiki
    restart: unless-stopped
    ports:
      - "3300:3000"
```

# Matomo

## Overview

- no dark theme
- pretty complex
- can be private when setup that way
- lots of stats & metrics

<br>

- [Homepage](https://matomo.org)
- [Github repo](https://github.com/matomo-org/docker)


## `docker-compose.yml`
```yml
services:
  db:
    image: mariadb
    command: --max-allowed-packet=64MB
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: matomo
      MYSQL_USER: matomo
      MYSQL_PASSWORD: matomo
      MYSQL_DATABASE: matomo
      MARIADB_AUTO_UPGRADE: 1
      MARIADB_INITDB_SKIP_TZINFO: 1
    volumes:
      - ./db:/var/lib/mysql

  app:
    image: matomo
    restart: unless-stopped
    environment:
      MATOMO_DATABASE_HOST: db
      MATOMO_DATABASE_ADAPTER: mysql
      MATOMO_DATABASE_TABLES_PREFIX:
      MATOMO_DATABASE_USERNAME: matomo
      MATOMO_DATABASE_PASSWORD: matomo
      MATOMO_DATABASE_DBNAME: matomo
    ports:
      - 3000:80
    volumes:
      - ./data:/var/www/html
```

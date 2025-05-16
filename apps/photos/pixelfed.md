# Pixelfed

Instagram-like photo stream.

<br>

- [Homepage](https://pixelfed.org/)
- [Github repo](https://github.com/pixelfed/pixelfed)
- [Docs](https://docs.pixelfed.org/)

## Setup

First create `.env` file like this one: https://github.com/pixelfed/pixelfed/blob/dev/.env.docker

## docker-compose.yml
```yml
services:
  app:
    image: pixelfed
    restart: unless-stopped
    env_file:
      - ./.env.docker
    volumes:
      - "app-storage:/var/www/storage"
      - "app-bootstrap:/var/www/bootstrap"
      - "./.env.docker:/var/www/.env"
    networks:
      - external
      - internal
    ports:
    - "8080:80"
    depends_on:
      - db
      - redis

  worker:
    image: pixelfed
    restart: unless-stopped
    env_file:
      - ./.env.docker
    volumes:
      - "app-storage:/var/www/storage"
      - "app-bootstrap:/var/www/bootstrap"
    networks:
      - external
      - internal
    command: gosu www-data php artisan horizon
    depends_on:
      - db
      - redis

  db:
    image: mysql:8.0
    restart: unless-stopped
    networks:
      - internal
    command: --default-authentication-plugin=mysql_native_password
    environment:
      - MYSQL_DATABASE=pixelfed
      - MYSQL_USER=${DB_USERNAME}
      - MYSQL_PASSWORD=${DB_PASSWORD}
      - MYSQL_RANDOM_ROOT_PASSWORD=true
    volumes:
      - "db-data:/var/lib/mysql"

  redis:
    image: redis:5-alpine
    restart: unless-stopped
    volumes:
      - "redis-data:/data"
    networks:
      - internal

volumes:
  db-data:
  redis-data:
  app-storage:
  app-bootstrap:

networks:
  internal:
    internal: true
  external:
    driver: bridge
```

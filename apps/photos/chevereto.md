# Chevereto

Mature and trusted self-hosted image and video hosting solution since 2009. Create your own Flickr/Imgur-style media sharing platform with full control over your content and rules.

- [Github repo](https://github.com/chevereto/chevereto)
- [Homepage](https://chevereto.com/)
- [Docs](https://v4-docs.chevereto.com/guides/docker/pure-docker.html)

## docker-compose.yml

```yml
services:
  database:
    image: mariadb:jammy
    networks:
      - chevereto
    volumes:
      - database:/var/lib/mysql
    restart: always
    healthcheck:
      test: ["CMD", "healthcheck.sh", "--su-mysql", "--connect"]
      interval: 10s
      timeout: 5s
      retries: 3
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: chevereto
      MYSQL_USER: chevereto
      MYSQL_PASSWORD: user_database_password

  php:
    image: chevereto/chevereto:latest # tweak with target image to run
    networks:
      - chevereto
    volumes:
      - storage:/var/www/html/images/
      # - app:/var/www/html/ # uncomment when using CHEVERETO_SERVICING=server
    restart: always
    depends_on:
      database:
        condition: service_healthy
    expose:
      - 80
    environment:
      CHEVERETO_DB_HOST: database
      CHEVERETO_DB_USER: chevereto
      CHEVERETO_DB_PASS: user_database_password
      CHEVERETO_DB_PORT: 3306
      CHEVERETO_DB_NAME: chevereto
      CHEVERETO_HOSTNAME: hostname.com
      CHEVERETO_HOSTNAME_PATH: /
      CHEVERETO_HTTPS: 0
      CHEVERETO_MAX_POST_SIZE: 2G
      CHEVERETO_MAX_UPLOAD_SIZE: 2G
      # CHEVERETO_SERVICING: server # uncomment to enable application filesystem upgrades

volumes:
  database:
  storage:
  # app: # uncomment when using CHEVERETO_SERVICING=server

networks:
  chevereto:
```

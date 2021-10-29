# Wallabag

- A self hostable application for saving web pages, freely.
- Alternative to Pocket.
- Has mobile apps

<br>

- [Homepage](https://www.wallabag.it/en)
- [Github repo](https://github.com/wallabag/docker)

![Screenshot](wallabag.jpg)


## docker-compose.yml
```yml
version: '3'
services:
  wallabag:
    image: wallabag/wallabag
    environment:
      - MYSQL_ROOT_PASSWORD=wallaroot
      - SYMFONY__ENV__DATABASE_DRIVER=pdo_mysql
      - SYMFONY__ENV__DATABASE_HOST=db
      - SYMFONY__ENV__DATABASE_PORT=3306
      - SYMFONY__ENV__DATABASE_NAME=wallabag
      - SYMFONY__ENV__DATABASE_USER=wallabag
      - SYMFONY__ENV__DATABASE_PASSWORD=wallapass
      - SYMFONY__ENV__DATABASE_CHARSET=utf8mb4
      - SYMFONY__ENV__MAILER_HOST=127.0.0.1
      - SYMFONY__ENV__MAILER_USER=~
      - SYMFONY__ENV__MAILER_PASSWORD=~
      - SYMFONY__ENV__FROM_EMAIL=wallabag@example.com
      - SYMFONY__ENV__DOMAIN_NAME=https://your-wallabag-url-instance.com
    ports:
      - "3123:80"
    volumes:
      - ./images:/var/www/wallabag/web/assets/images
      - ./material.css:/var/www/wallabag/web/wallassets/material.css  # dark theme hack
  db:
    image: mariadb
    environment:
      - MYSQL_ROOT_PASSWORD=wallaroot
    volumes:
      - ./data:/var/lib/mysql
  redis:
    image: redis:alpine
```

Default login is `wallabag`:`wallabag`.


## Tips & Tricks

#### [A hack to get the dark theme](https://github.com/wallabag/wallabag/issues/1521#issuecomment-720541571):

1. Create a file `material.css` containing the original material theme CSS + the CSS provided by @STaRDoGG (in the link above) - [material.css.zip](wallabag-material.css.zip).
2. In docker-compose mount the file like so:
    ```yml
      - ./material.css:/var/www/wallabag/web/wallassets/material.css
    ```
3. Once the app starts - reload browser without cache (cmd/ctrl+r)

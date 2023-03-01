# Baikal

lightweight CalDAV+CardDAV server. It offers an extensive web interface with easy management of users, address books and calendars. It is fast and simple to install and only needs a basic php capable server. The data can be stored in a MySQL or a SQLite database.

<br>

- [Github repo](https://github.com/ckulka/baikal-docker)
- [Homepage](https://sabre.io/baikal/)

## docker-compose

```yml
---
services:
  baikal:
    image: ckulka/baikal:nginx
    depends_on:
      - baikal-db
    restart: unless-stopped
    ports:
      - "8456:80"
    volumes:
      - ./config:/var/www/baikal/config
      - ./Specific:/var/www/baikal/Specific
    networks:
      - baikal-network

  baikal-db:
    image: mariadb:latest
    restart: unless-stopped
    volumes:
      - ./mysql-data:/var/lib/mysql
      - ./mysql:/etc/mysql/conf.d
    ports:
      - "4406:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
      - MYSQL_DATABASE=baikal-db
      - MYSQL_USER=user
      - MYSQL_PASSWORD=password
    networks:
      - baikal-network

networks:
 baikal-network:
```

## Tips & tricks

### Create and chown  the mounting folders before running docker-compose up

`mkdir -p config Specific`

And

`chown -R 101:101 config Specific`



### Create admin and users(s)

From the web panel setup admin and auth method

Enable mysql

And under database host fill the host lan IP with the database port as mapped in the docker-compose  e.g. 192.168.1.24:4406

Then create a user

### Android caldav carddav client

On DAVx5 use Base URL: /dav.php/ 

(e.g. https://server.example/dav.php/)

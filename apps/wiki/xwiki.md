# XWiki

- Quite close to Confluence
- UI looks a bit dated, but functional and configurable

<br>

- [Homepage](https://www.xwiki.org)
- [Github repo](https://github.com/xwiki)
- [Docker installation](https://github.com/xwiki-contrib/docker-xwiki/blob/master/README.md#using-docker-compose)


## Prerequisities
First run these:
```sh
wget https://raw.githubusercontent.com/xwiki-contrib/docker-xwiki/master/11/mysql-tomcat/mysql/xwiki.cnf
wget https://raw.githubusercontent.com/xwiki-contrib/docker-xwiki/master/11/mysql-tomcat/mysql/init.sql
```

## docker-compose.yml
```yml
version: '2'
services:
  web:
    image: "xwiki:lts-mysql-tomcat"
    container_name: xwiki-mysql-tomcat-web
    depends_on:
      - db
    ports:
      - "3123:8080"
    environment:
      - DB_USER=xwiki
      - DB_PASSWORD=xwiki
      - DB_HOST=xwiki-mysql-db
    volumes:
      - ./data:/usr/local/xwiki
  db:
    image: "mysql:5.7"
    container_name: xwiki-mysql-db
    volumes:
      - ./xwiki.cnf:/etc/mysql/conf.d/xwiki.cnf
      - ./db:/var/lib/mysql
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    environment:
      - MYSQL_ROOT_PASSWORD=xwiki
      - MYSQL_USER=xwiki
      - MYSQL_PASSWORD=xwiki
      - MYSQL_DATABASE=xwiki
```

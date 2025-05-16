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
wget https://raw.githubusercontent.com/xwiki-contrib/docker-xwiki/master/12/mysql-tomcat/mysql/xwiki.cnf
wget https://raw.githubusercontent.com/xwiki-contrib/docker-xwiki/master/12/mysql-tomcat/mysql/init.sql
```

## docker-compose.yml
```yml
services:
  db:
    image: mysql:5.7
    container_name: xwiki-db
    volumes:
      - ./xwiki.cnf:/etc/mysql/conf.d/xwiki.cnf
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
      - ./db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=xwiki
      - MYSQL_USER=xwiki
      - MYSQL_PASSWORD=xwiki
      - MYSQL_DATABASE=xwiki
  xwiki:
    image: xwiki:lts-mysql-tomcat
    container_name: xwiki
    depends_on:
      - db
    ports:
      - "3123:8080"
    environment:
      - DB_USER=xwiki
      - DB_PASSWORD=xwiki
      - DB_HOST=db
    volumes:
      - ./data:/usr/local/xwiki
```

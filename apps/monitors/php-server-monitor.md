# PhpServerMonitor

- Very basic and minimalist
- Various monitors (ping, tcp & http service requests)
- Has Pushover notifications

<br>


- [Github repo](https://github.com/phpservermon/phpservermon)
- [DockerHub repo](https://hub.docker.com/r/alwynpan/phpservermonitor)


## docker-compose.yml
```yml
services:
 psm:
   image: alwynpan/phpservermonitor
   container_name: php-server-monitor
   restart: unless-stopped
   ports:
     - 3123:80
   depends_on:
     - db
   environment:
     - DATABASE_HOST=db
     - DATABASE_PORT=3306
     - DATABASE_NAME=psm
     - DATABASE_USER=psm
     - DATABASE_PASSWORD=psm
     - BASE_URL=http://192.168.1.10:3123
     - CHECK_INTERVAL=1
     - TIMEOUT=10
     - DEBUG=true

 db:
   image: mysql:5.7
   container_name: php-server-monitor-db
   restart: unless-stopped
   ports:
     - 3306:3306
   environment:
     - MYSQL_ROOT_PASSWORD=top_secret
     - MYSQL_DATABASE=psm
     - MYSQL_USER=psm
     - MYSQL_PASSWORD=psm
   volumes:
     - ./data/:/var/lib/mysql
```

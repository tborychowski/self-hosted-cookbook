# Confluence

- Not free. Used to be $10 for up to 10 users. From Feb 2021 the Server edition will stop selling new licenses.
- Most feature rich and easy to use by less tech-savvy people.
- Stable core and well tested (that is not to say it has no bugs :smile: e.g. code macro strips leading spaces on paste).
- Slow.

<br>

- [Homepage](https://hub.docker.com/r/atlassian/confluence-server)
- [DockerHub repo](https://hub.docker.com/r/atlassian/confluence-server)


## docker-compose.yml
```yml
services:
  confluence:
    image: atlassian/confluence-server
    container_name: confluence
    environment:
      - TZ=Europe/Dublin
      - JVM_MINIMUM_MEMORY=2048m
      - JVM_MAXIMUM_MEMORY=8192m
      - ATL_JDBC_USER=dbuser1
      - ATL_JDBC_PASSWORD=123456
      - ATL_DB_TYPE=postgresql
      - ATL_PROXY_NAME=confluence.example.com
      - ATL_PROXY_PORT=443
      - ATL_TOMCAT_SCHEME=https
      - ATL_TOMCAT_SECURE=true
    volumes:
      - ./data:/var/atlassian/application-data/confluence
    ports:
      - 8090:8090
      - 8091:8091
    restart: unless-stopped
    depends_on:
      - db

  db:
    container_name: confluence-postgres
    image: postgres:12.4
    command: postgres -c 'shared_buffers=128MB' -c 'max_connections=125'
    restart: unless-stopped
    volumes:
      - ./database:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=dbuser1
      - POSTGRES_PASSWORD=123456
```

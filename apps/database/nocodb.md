# NocoDB

- Open Source Airtable Alternative - turns any MySQL, Postgres, SQLite into a Spreadsheet with REST APIs.
- quite powerful & lots of options
- too much eye candy animations

<br>

- [Homepage](https://nocodb.com)
- [Docs](https://docs.nocodb.com)
- [Github Repo](https://docs.nocodb.com)


## docker-compose.yml
```yml
---
services:
  root_db:
    image: mysql:5.7
    container_name: nocodb-db
    restart: unless-stopped
    healthcheck:
      test: [ "CMD", "mysqladmin" ,"ping", "-h", "localhost" ]
      timeout: 20s
      retries: 10
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: root_db
      MYSQL_USER: noco
      MYSQL_PASSWORD: password
    volumes:
      - ./db_data:/var/lib/mysql

  nocodb:
    image: nocodb/nocodb:latest
    container_name: nocodb
    depends_on:
      root_db:
        condition: service_healthy
    restart: unless-stopped
    environment:
      NC_DB: "mysql2://root_db:3306?u=noco&p=password&d=root_db"
    ports:
      - "3123:8080"
    volumes:
      - ./nc_data:/usr/app/data
```

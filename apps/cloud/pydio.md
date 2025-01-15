# Pydio Cells
- Overly complex and... weird.
- Has mobile apps.

<br>

- [Homepage](https://pydio.com/en/features/pydio-cells-overview)
- [Github repo](https://github.com/pydio/cells)
- [DockerHub repo](https://hub.docker.com/r/pydio/cells/)
- [Docs](https://pydio.com/en/docs/administration-guides)


## docker-compose.yml
```yml
---
services:
  cells:
    image: pydio/cells:latest
    container_name: pydio
    hostname: cells
    restart: unless-stopped
    ports:
      - "3060:8080"
    environment:
      - CELLS_NO_TLS=0
      - CELLS_BIND=0.0.0.0:8080
      - CELLS_EXTERNAL=pydio.domain.com
      - EXTERNAL_BIND=pydio.domain.com
    volumes:
      - "./datadir:/var/cells/data"
      - "./workingdir:/var/cells"

  mysql:
    image: mysql:5.7
    container_name: pydio-db
    restart: unless-stopped
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: cells
      MYSQL_USER: pydio
      MYSQL_PASSWORD: secret
    command: [mysqld, --character-set-server=utf8mb4, --collation-server=utf8mb4_unicode_ci]
    volumes:
      - "./mysqldir:/var/lib/mysql"
```

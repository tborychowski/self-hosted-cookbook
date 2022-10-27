# PenPot
Open Source design and prototyping platform for cross-domain teams

- like Figma but poorer.

<br>

- [Homepage](https://penpot.app)
- [Github repo](https://github.com/penpot/penpot)
- [Docs](https://help.penpot.app/technical-guide/getting-started/#install-with-docker)


## You can get the 2 required files directly from github:
```sh
wget https://raw.githubusercontent.com/penpot/penpot/main/docker/images/docker-compose.yaml
wget https://raw.githubusercontent.com/penpot/penpot/main/docker/images/config.env
```

Or copy & paste from here:

## docker-compose.yml
```yml
---
services:
  penpot-frontend:
    image: penpotapp/frontend:latest
	container_name: penpot-frontend
    ports:
      - 9001:80
    volumes:
      - ./data:/opt/data
    env_file:
      - config.env
    depends_on:
      - penpot-backend
      - penpot-exporter

  penpot-backend:
    image: penpotapp/backend:latest
    volumes:
      - ./data:/opt/data
    depends_on:
      - penpot-postgres
      - penpot-redis
    env_file:
      - config.env

  penpot-exporter:
    image: penpotapp/exporter:latest
	container_name: penpot-exporter
    env_file:
      - config.env
    environment:
      # Don't touch it; this uses internal docker network to communicate with the frontend.
      - PENPOT_PUBLIC_URI=http://penpot-frontend

  penpot-postgres:
    image: "postgres:14"
	container_name: penpot-db
    restart: unless-stopped
    stop_signal: SIGINT
    environment:
      - POSTGRES_INITDB_ARGS=--data-checksums
      - POSTGRES_DB=penpot
      - POSTGRES_USER=penpot
      - POSTGRES_PASSWORD=penpot
    volumes:
      - ./db:/var/lib/postgresql/data

  penpot-redis:
    image: redis:7
	container_name: penpot-redis
    restart: unless-stopped
```

## config.env
Shortened version, just the minimum to test things out, with telemetry disabled.

```ini
## Should be set to the public domain where penpot is going to be served.
##
## NOTE: If you are going to serve it under different domain than
## 'localhost' without HTTPS, consider setting the
## `disable-secure-session-cookies' flag on the 'PENPOT_FLAGS' setting.

PENPOT_PUBLIC_URI=http://localhost:9001

PENPOT_FLAGS=enable-registration enable-login disable-email-verification
PENPOT_HTTP_SERVER_HOST=0.0.0.0
PENPOT_DATABASE_URI=postgresql://penpot-postgres/penpot
PENPOT_DATABASE_USERNAME=penpot
PENPOT_DATABASE_PASSWORD=penpot
PENPOT_REDIS_URI=redis://penpot-redis/0
PENPOT_ASSETS_STORAGE_BACKEND=assets-fs
PENPOT_STORAGE_ASSETS_FS_DIRECTORY=/opt/data/assets
PENPOT_TELEMETRY_ENABLED=false
# PENPOT_REGISTRATION_DOMAIN_WHITELIST=""
```

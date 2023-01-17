# LibrePhotos

Actively maintained fork of [OwnPhotos](https://github.com/hooram/ownphotos).
- "Dated" UI
- Lots of stats and "AI" features (face & object detection, smart tags, etc.)
- Local folder or NextCloud sync
- Lack of video support
- Lack of upload via UI

<br>

- [Github repo](https://github.com/LibrePhotos/librephotos)
- [Demo](https://demo2.librephotos.com/login) (Login with: `demo`, `demo1234`)


## Prerequisites
Create local folders:
```
mkdir photos thumbnails cache logs data
```

## Setup

## .env
```ini
# Comma delimited list of folder patterns to ignore when scanning for photos
skipPatterns=@eaDir,#recycle

myPhotos=./photos
proMedia=./thumbnails
cachedir=./cache
logLocation=./logs
dbLocation=./data
timeZone=Europe/Dublin
httpPort=3000

# Admin
userName=admin
userPass=admin
adminEmail=admin@example.com

# Secret, e.g. `openssl rand -base64 32`
shhhhKey=29hfFSZBmgn6b00zGgZL+12la64qb29Hel4eJwayhvA=

# ------------------------------------------------------------------------------------------------
# optional: you do not have to change any of the below.

# Get a Map box API Key https://account.mapbox.com/auth/signup/
mapApiKey=

# only the dev branch is currently supported
tag=dev

# database.
dbName=ownphotos
dbUser=docker
dbPass=AaAa1234

# This setting can dramatically affect how resource-intensive and the speed of scanning photos
# A positive integer generally in the 2-4 x $(NUM_CORES) range.
# You’ll want to vary this a bit to find the best for your particular workload.
# Each worker needs 800MB of RAM. Change at your own will. Default is 2.
gunniWorkers=2

# Gunicorn worker timeout seconds (ensure that one bad request doesn't stall other requests forever)
workerTimeOut=1800
```

## docker-compose.yml
```yml
---
version: '2.1'
services:
  proxy:
    image: reallibrephotos/librephotos-proxy:${tag}
    tty: true
    container_name: librephotos-proxy
    restart: unless-stopped
    ports:
      - ${httpPort}:80

  librephotos-db:
    image: postgres
    container_name: librephotos-db
    restart: unless-stopped
    environment:
      - POSTGRES_USER=${dbUser}
      - POSTGRES_PASSWORD=${dbPass}
      - POSTGRES_DB=${dbName}
    volumes:
      - ${dbLocation}:/var/lib/postgresql/data
    command: postgres -c fsync=off -c synchronous_commit=off -c full_page_writes=off -c random_page_cost=1.0
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -d $dbName -U $dbUser']
      interval: 5s
      timeout: 5s
      retries: 5

  frontend:
    image: reallibrephotos/librephotos-frontend:${tag}
    container_name: librephotos-frontend
    restart: unless-stopped
    tty: true

  backend:
    image: reallibrephotos/librephotos:${tag}
    container_name: librephotos-backend
    restart: unless-stopped
    volumes:
      - ${myPhotos}:/data
      - ${proMedia}:/code/protected_media
      - ${logLocation}:/code/logs
      - ${cachedir}:/root/.cache

    environment:
      - SECRET_KEY=${shhhhKey}
      - BACKEND_HOST=backend
      - ADMIN_EMAIL=${adminEmail}
      - ADMIN_USERNAME=${userName}
      - ADMIN_PASSWORD=${userPass}
      - DEBUG=true
      - DB_BACKEND=postgresql
      - DB_NAME=${dbName}
      - DB_USER=${dbUser}
      - DB_PASS=${dbPass}
      - DB_HOST=librephotos-db
      - DB_PORT=5432
      - REDIS_HOST=librephotos-redis
      - REDIS_PORT=6379
      - MAPBOX_API_KEY=${mapApiKey}
      - TIME_ZONE=${timeZone}
      - WEB_CONCURRENCY=${gunniWorkers}
      - WORKER_TIMEOUT=${workerTimeOut}
      - SKIP_PATTERNS=${skipPatterns}

    # Wait for Postgres
    depends_on:
      librephotos-db:
        condition: service_healthy

  librephotos-redis:
    image: redis:alpine
    container_name: librephotos-redis
    restart: unless-stopped
```
## Librephotos backuo/restore

Docker offers volumes so /data and /code/protected_media are safely mounted on host. Simply rsync backup of these dirs 

posstgres db backup

Usefull to avoid a full scan 

`docker ps`

Grab postgres container id e.g.40b12de38945

Backup

`docker exec -t your-db-container pg_dumpall -c -U your-db-user > dump_$(date +%Y-%m-%d_%H_%M_%S).sql`

A file like dump_20-11-2022_00_55_26.sql is created

Restore

`cat your_dump.sql | docker exec -i your-db-container psql -U your-db-user -d your-db-name`

*Note the -U dbuser (default is docker for librephotos)
and the -d bdname (default librephotos for librephotos)

Put that on a crontab
And there you go you have periodic db snapshots

# Paperless-ngx
- decent ui that is also usable on mobile
- lots of filters
- very cool auto tagging, auto categorisation
- trash
- sometimes textual pdfs are OCR-ed, and show gibberish in preview

<br>

- [Github repo](https://github.com/paperless-ngx/paperless-ngx)
- [Docs](https://paperless-ngx.readthedocs.io/en/latest/)


## .env
```ini
COMPOSE_PROJECT_NAME=paperless
```

## docker-compose.env
```ini
# The container installs English, German, Italian, Spanish and French by default.
# available languages:
# https://packages.debian.org/search?keywords=tesseract-ocr-&searchon=names&suite=buster
# PAPERLESS_OCR_LANGUAGES=eng

# This is required to expose Paperless-ngx on a public domain
PAPERLESS_URL=https://paperless.domain.com
PAPERLESS_SECRET_KEY=changeme
PAPERLESS_TIME_ZONE=Europe/Dublin

# Default language to use for OCR
PAPERLESS_OCR_LANGUAGE=eng
```

## docker-compose.yml
```yml
services:
  broker:
    image: docker.io/library/redis:6.0
    container_name: paperless-redis
    restart: unless-stopped
    volumes:
      - ./redis:/data

  gotenberg:
    image: docker.io/gotenberg/gotenberg:7.4
    container_name: paperless-gotenberg
    restart: unless-stopped
    command:
      - "gotenberg"
      - "--chromium-disable-routes=true"

  tika:
    image: ghcr.io/paperless-ngx/tika:latest
    container_name: paperless-tika
    restart: unless-stopped

  webserver:
    image: ghcr.io/paperless-ngx/paperless-ngx:latest
    container_name: paperless-ngx
    restart: unless-stopped
    depends_on:
      - broker
      - gotenberg
      - tika
    healthcheck:
      test: ["CMD", "curl", "-fs", "-S", "--max-time", "2", "http://localhost:8000"]
      interval: 30s
      timeout: 10s
      retries: 5
    env_file: docker-compose.env
    environment:
      PAPERLESS_REDIS: redis://broker:6379
      PAPERLESS_TIKA_ENABLED: 1
      PAPERLESS_TIKA_GOTENBERG_ENDPOINT: http://gotenberg:3000
      PAPERLESS_TIKA_ENDPOINT: http://tika:9998
      PAPERLESS_TRASH_DIR: /usr/src/paperless/trash
      PAPERLESS_OCR_MODE: skip_noarchive

    ports:
      - 8000:8000
    volumes:
      - ./data:/usr/src/paperless/data
      - ./media:/usr/src/paperless/media
      - ./export:/usr/src/paperless/export
      - ./consume:/usr/src/paperless/consume
      - ./trash:/usr/src/paperless/trash
```

## First run these:
```sh
# fetch the docker image
docker-compose pull

# create admin user
docker-compose run --rm webserver createsuperuser

# start
docker-compose up -d
```

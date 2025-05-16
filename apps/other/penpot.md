# PenPot
Open Source design and prototyping platform for cross-domain teams

- like Figma but poorer.

<br>

- [Homepage](https://penpot.app)
- [Github repo](https://github.com/penpot/penpot)
- [Docs](https://help.penpot.app/technical-guide/getting-started/#install-with-docker)


## You can get the required file directly from github:
```sh
wget https://raw.githubusercontent.com/penpot/penpot/main/docker/images/docker-compose.yaml
```
Or copy & paste from here:


## docker-compose.yml
```yml
## You can read more about all available flags and other
## environment variables here:
## https://help.penpot.app/technical-guide/configuration/#advanced-configuration
#
# WARNING: if you're exposing Penpot to the internet, you should remove the flags
# 'disable-secure-session-cookies' and 'disable-email-verification'
x-flags: &penpot-flags
  PENPOT_FLAGS: disable-email-verification enable-smtp enable-prepl-server disable-secure-session-cookies

x-uri: &penpot-public-uri
  PENPOT_PUBLIC_URI: http://192.168.1.10:9001

x-body-size: &penpot-http-body-size
  # Max body size (30MiB); Used for plain requests, should never be
  # greater than multi-part size
  PENPOT_HTTP_SERVER_MAX_BODY_SIZE: 31457280

  # Max multipart body size (350MiB)
  PENPOT_HTTP_SERVER_MAX_MULTIPART_BODY_SIZE: 367001600


services:

  penpot-frontend:
    image: "penpotapp/frontend:latest"
    restart: unless-stopped
    depends_on:
      - penpot-backend
      - penpot-exporter
    environment:
      << : [*penpot-flags, *penpot-http-body-size]
    ports:
      - 9001:8080
    volumes:
      - ./assets:/opt/data/assets


  penpot-backend:
    image: "penpotapp/backend:latest"
    restart: unless-stopped
    depends_on:
      penpot-postgres:
        condition: service_healthy
      penpot-redis:
        condition: service_healthy
    environment:
      << : [*penpot-flags, *penpot-public-uri, *penpot-http-body-size]
      PENPOT_SECRET_KEY: 93ac8dc5eb2bb583bd8f827bd31be520dd32ead00657237d66662013770ee45c
      PENPOT_DATABASE_URI: postgresql://penpot-postgres/penpot
      PENPOT_DATABASE_USERNAME: penpot
      PENPOT_DATABASE_PASSWORD: penpot
      PENPOT_REDIS_URI: redis://penpot-redis/0
      PENPOT_ASSETS_STORAGE_BACKEND: assets-fs
      PENPOT_STORAGE_ASSETS_FS_DIRECTORY: /opt/data/assets
      PENPOT_TELEMETRY_ENABLED: false
      PENPOT_TELEMETRY_REFERER: compose

      ## Example SMTP/Email configuration. By default, emails are sent to the mailcatch
      ## service, but for production usage it is recommended to setup a real SMTP
      ## provider. Emails are used to confirm user registrations & invitations. Look below
      ## how the mailcatch service is configured.
      PENPOT_SMTP_DEFAULT_FROM: no-reply@example.com
      PENPOT_SMTP_DEFAULT_REPLY_TO: no-reply@example.com
      PENPOT_SMTP_HOST: penpot-mailcatch
      PENPOT_SMTP_PORT: 1025
      PENPOT_SMTP_USERNAME:
      PENPOT_SMTP_PASSWORD:
      PENPOT_SMTP_TLS: false
      PENPOT_SMTP_SSL: false

    volumes:
      - ./assets:/opt/data/assets


  penpot-exporter:
    image: "penpotapp/exporter:latest"
    restart: unless-stopped
    depends_on:
      penpot-redis:
        condition: service_healthy
    environment:
      PENPOT_PUBLIC_URI: http://penpot-frontend:8080
      PENPOT_REDIS_URI: redis://penpot-redis/0


  penpot-postgres:
    image: "postgres:15"
    restart: unless-stopped
    stop_signal: SIGINT
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U penpot"]
      interval: 2s
      timeout: 10s
      retries: 5
      start_period: 2s
    environment:
      - POSTGRES_INITDB_ARGS=--data-checksums
      - POSTGRES_DB=penpot
      - POSTGRES_USER=penpot
      - POSTGRES_PASSWORD=penpot
    volumes:
      - ./db:/var/lib/postgresql/data


  penpot-redis:
    image: redis:7.2
    restart: unless-stopped

    healthcheck:
      test: ["CMD-SHELL", "redis-cli ping | grep PONG"]
      interval: 1s
      timeout: 3s
      retries: 5
      start_period: 3s


  ## A mailcatch service, used as temporal SMTP server. You can access via HTTP to the
  ## port 1080 for read all emails the penpot platform has sent. Should be only used as a
  ## temporal solution while no real SMTP provider is configured.
  penpot-mailcatch:
    image: sj26/mailcatcher:latest
    restart: unless-stopped
    expose:
      - '1025'
    ports:
      - "1080:1080"

```

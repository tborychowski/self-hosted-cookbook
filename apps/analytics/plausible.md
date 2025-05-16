# Plausible

## Overview

- very simple and easy to setup
- very private (doesn't track much, anonymises data)
- nice dark theme

<br>

- [Homepage](https://plausible.io)
- [Docs](https://plausible.io/docs)
- [Github repo](https://github.com/plausible/analytics)


## `schema.postgresql.sql`

```env
SECRET_KEY_BASE=1234567890
BASE_URL=https://plausible.domain.com
ENABLE_EMAIL_VERIFICATION=false

MAILER_EMAIL=plausible@domain.com
SMTP_HOST_ADDR=smtp.mail.com
SMTP_HOST_PORT=465
SMTP_USER_NAME=user@domain.com
SMTP_USER_PWD=password
SMTP_HOST_SSL_ENABLED=true
SMTP_RETRIES=2
```

## `docker-compose.yml`

```yml
services:
  mail:
    image: bytemark/smtp
    container_name: smtp
    restart: unless-stopped

  plausible_db:
    image: postgres:14-alpine
    container_name: plausible-postgres
    restart: unless-stopped
    volumes:
      - ./db:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=postgres

  plausible_events_db:
    image: clickhouse/clickhouse-server:22.6-alpine
    container_name: plausible-events-db
    restart: unless-stopped
    ulimits:
      nofile:
        soft: 262144
        hard: 262144
    volumes:
      - ./event-data:/var/lib/clickhouse
      - ./clickhouse/clickhouse-config.xml:/etc/clickhouse-server/config.d/logging.xml:ro
      - ./clickhouse/clickhouse-user-config.xml:/etc/clickhouse-server/users.d/logging.xml:ro

  plausible:
    image: plausible/analytics:latest
    container_name: plausible
    restart: unless-stopped
    command: sh -c "sleep 10 && /entrypoint.sh db createdb && /entrypoint.sh db migrate && /entrypoint.sh run"
    depends_on:
      - plausible_db
      - plausible_events_db
      - mail
    env_file:
      - plausible-conf.env
    ports:
      - 3123:8000
```

# Budibase
- Build internal apps in minutes.
- This is a "meta" app, that allows to quickly create a database, fill it with data, and create a simple app that would read/write data.

<br>

- [Homepage](https://budibase.com)
- [Github repo](https://github.com/Budibase/budibase)
- [Docs](https://docs.budibase.com/docs)


## .env
```ini
MAIN_PORT=10000

# These should be updated
JWT_SECRET=testsecret
MINIO_ACCESS_KEY=budibase
MINIO_SECRET_KEY=budibase
COUCH_DB_PASSWORD=budibase
COUCH_DB_USER=budibase
REDIS_PASSWORD=budibase
INTERNAL_API_KEY=budibase

# This section contains variables that do not need to be altered under normal circumstances
APP_PORT=4002
WORKER_PORT=4003
MINIO_PORT=4004
COUCH_DB_PORT=4005
REDIS_PORT=6379
BUDIBASE_ENVIRONMENT=PRODUCTION

# An admin user can be automatically created initially if these are set
BB_ADMIN_USER_EMAIL=admin@domain.com
BB_ADMIN_USER_PASSWORD=admin
```

## docker-compose.yml
```yml
---
services:
  app-service:
    restart: unless-stopped
    image: budibase.docker.scarf.sh/budibase/apps
    container_name: bbapps
    environment:
      SELF_HOSTED: 1
      COUCH_DB_URL: http://${COUCH_DB_USER}:${COUCH_DB_PASSWORD}@couchdb-service:5984
      WORKER_URL: http://worker-service:4003
      MINIO_URL: http://minio-service:9000
      MINIO_ACCESS_KEY: ${MINIO_ACCESS_KEY}
      MINIO_SECRET_KEY: ${MINIO_SECRET_KEY}
      INTERNAL_API_KEY: ${INTERNAL_API_KEY}
      BUDIBASE_ENVIRONMENT: ${BUDIBASE_ENVIRONMENT}
      PORT: 4002
      JWT_SECRET: ${JWT_SECRET}
      LOG_LEVEL: info
      SENTRY_DSN: https://a34ae347621946bf8acded18e5b7d4b8@o420233.ingest.sentry.io/5338131
      ENABLE_ANALYTICS: "false"
      REDIS_URL: redis-service:6379
      REDIS_PASSWORD: ${REDIS_PASSWORD}
      BB_ADMIN_USER_EMAIL: ${BB_ADMIN_USER_EMAIL}
      BB_ADMIN_USER_PASSWORD: ${BB_ADMIN_USER_PASSWORD}
    depends_on:
      - worker-service
      - redis-service


  worker-service:
    restart: unless-stopped
    image: budibase.docker.scarf.sh/budibase/worker
    container_name: bbworker
    environment:
      SELF_HOSTED: 1
      PORT: 4003
      CLUSTER_PORT: ${MAIN_PORT}
      JWT_SECRET: ${JWT_SECRET}
      MINIO_ACCESS_KEY: ${MINIO_ACCESS_KEY}
      MINIO_SECRET_KEY: ${MINIO_SECRET_KEY}
      MINIO_URL: http://minio-service:9000
      APPS_URL: http://app-service:4002
      COUCH_DB_USERNAME: ${COUCH_DB_USER}
      COUCH_DB_PASSWORD: ${COUCH_DB_PASSWORD}
      COUCH_DB_URL: http://${COUCH_DB_USER}:${COUCH_DB_PASSWORD}@couchdb-service:5984
      SENTRY_DSN: https://a34ae347621946bf8acded18e5b7d4b8@o420233.ingest.sentry.io/5338131
      INTERNAL_API_KEY: ${INTERNAL_API_KEY}
      REDIS_URL: redis-service:6379
      REDIS_PASSWORD: ${REDIS_PASSWORD}
    depends_on:
      - redis-service
      - minio-service
      - couch-init


  minio-service:
    restart: unless-stopped
    image: minio/minio
    environment:
      MINIO_ACCESS_KEY: ${MINIO_ACCESS_KEY}
      MINIO_SECRET_KEY: ${MINIO_SECRET_KEY}
      MINIO_BROWSER: "off"
    command: server /data --console-address ":9001"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
      interval: 30s
      timeout: 20s
      retries: 3
    volumes:
      - ./minio:/data


  proxy-service:
    restart: unless-stopped
    ports:
      - "${MAIN_PORT}:10000"
    container_name: bbproxy
    image: budibase/proxy
    environment:
      - PROXY_RATE_LIMIT_WEBHOOKS_PER_SECOND=10
    depends_on:
      - minio-service
      - worker-service
      - app-service
      - couchdb-service


  couchdb-service:
    restart: unless-stopped
    image: ibmcom/couchdb3
    environment:
      - COUCHDB_PASSWORD=${COUCH_DB_PASSWORD}
      - COUCHDB_USER=${COUCH_DB_USER}
    volumes:
      - ./couchdb:/opt/couchdb/data


  couch-init:
    image: curlimages/curl
    environment:
      PUT_CALL: "curl -u ${COUCH_DB_USER}:${COUCH_DB_PASSWORD} -X PUT couchdb-service:5984"
    depends_on:
      - couchdb-service
    command: ["sh","-c","sleep 10 && $${PUT_CALL}/_users && $${PUT_CALL}/_replicator; fg;"]


  redis-service:
    restart: unless-stopped
    image: redis
    command: redis-server --requirepass ${REDIS_PASSWORD}
    volumes:
      - ./redis:/data
```

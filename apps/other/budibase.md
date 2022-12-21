# Budibase
- Build internal apps in minutes.
- This is a "meta" app, that allows to quickly create a database, fill it with data, and create a simple app that would read/write data.

<br>

- [Homepage](https://budibase.com)
- [Github repo](https://github.com/Budibase/budibase)
- [Docs](https://docs.budibase.com/docs)


## docker-compose.yml
```yml
---
services:
  budibase:
    image: budibase/budibase:latest
    container_name: budibase
    restart: unless-stopped
    environment:
      JWT_SECRET: lqOTTMi1wSphb7chGMfcHLCDZ1YHVFkdn6fnp0vj45U=
      MINIO_ACCESS_KEY: budibase
      MINIO_SECRET_KEY: budibase
      REDIS_PASSWORD: budibase
      COUCHDB_USER: budibase
      COUCHDB_PASSWORD: budibase
      INTERNAL_API_KEY: budibase
    ports:
      - "10000:80"
    volumes:
      - ./data:/data
```

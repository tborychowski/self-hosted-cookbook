# Hoarder
- Modern & simple UI
- Mobile apps
- Browser extensions (although no Safari)


<br>

- [Github repo](https://github.com/hoarder-app/hoarder)
- [Homepage](https://hoarder.app/)
- [Docs](https://docs.hoarder.app)


## .env
```ini
HOARDER_VERSION=latest
NEXTAUTH_SECRET=123
MEILI_MASTER_KEY=123
NEXTAUTH_URL=http://localhost:3123
```

## docker-compose.yml
```yml
---
services:

  redis:
    image: redis:7.2-alpine
	container_name: hoarder-redis
    restart: unless-stopped
    volumes:
      - ./redis:/data

  chrome:
    image: gcr.io/zenika-hub/alpine-chrome:123
	container_name: hoarder-chrome
    restart: unless-stopped
    command:
      - --no-sandbox
      - --disable-gpu
      - --disable-dev-shm-usage
      - --remote-debugging-address=0.0.0.0
      - --remote-debugging-port=9222
      - --hide-scrollbars

  meilisearch:
    image: getmeili/meilisearch:v1.6
	container_name: hoarder-meilisearch
    restart: unless-stopped
    env_file:
      - .env
    environment:
      MEILI_NO_ANALYTICS: "true"
    volumes:
      - ./meilisearch:/meili_data

  workers:
    image: ghcr.io/hoarder-app/hoarder-workers:latest
	container_name: hoarder-workers
    restart: unless-stopped
    env_file:
      - .env
    environment:
      REDIS_HOST: redis
      MEILI_ADDR: http://meilisearch:7700
      BROWSER_WEB_URL: http://chrome:9222
      DATA_DIR: /data
      # OPENAI_API_KEY: ...
    depends_on:
      web:
        condition: service_started
    volumes:
      - ./data:/data

  web:
    image: ghcr.io/hoarder-app/hoarder-web:latest
	container_name: hoarder
    restart: unless-stopped
    env_file:
      - .env
    environment:
      REDIS_HOST: redis
      MEILI_ADDR: http://meilisearch:7700
      DATA_DIR: /data
    ports:
      - 3123:3123
    volumes:
      - ./data:/data

```

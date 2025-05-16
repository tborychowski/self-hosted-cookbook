# Docmost

- Very simple and easy to use and setup.
- Minimalistic design, just enough functionality without the usual bloat!
- Nice dark & light themes.
- Probably the best OpenSource Confluence alternative out there!


<br>

- [Homepage](https://docmost.com)
- [Docs](https://docmost.com/docs/installation/)
- [GitHub repo](https://github.com/docmost/docmost)


## docker-compose.yml
```yml
services:
  docmost:
    image: docmost/docmost:latest
    container_name: docmost
    restart: unless-stopped
    depends_on:
      - db
      - redis
    environment:
      APP_URL: 'http://localhost:3000'
      APP_SECRET: 'Zw1JsbHQb/FM0evE/uc087s4t9X0OQNXnfg1jaWveA0='
      DATABASE_URL: 'postgresql://docmost:docmost@db:5432/docmost?schema=public'
      REDIS_URL: 'redis://redis:6379'
    ports:
      - "3000:3000"
    volumes:
      - ./data:/app/data/storage

  db:
    image: postgres:16-alpine
    container_name: docmost-postgres
    restart: unless-stopped
    environment:
      POSTGRES_DB: docmost
      POSTGRES_USER: docmost
      POSTGRES_PASSWORD: docmost
    volumes:
      - ./db_data:/var/lib/postgresql/data

  redis:
    image: redis:7.2-alpine
    container_name: docmost-redis
    restart: unless-stopped
    volumes:
      - ./redis_data:/data
```

# Joplin Server
Joplin is a free note taking app, available for desktop & mobile.
This is the Joplin sync server.

<br>

- [Homepage](https://joplinapp.org)
- [Github repo](https://github.com/laurent22/joplin/blob/dev/packages/server/README.md)

## docker-compose.yml
```yml
services:
  db:
    image: postgres:18
    container_name: joplin-db
    restart: unless-stopped
    environment:
      - POSTGRES_DB=joplin
      - POSTGRES_USER=joplin
      - POSTGRES_PASSWORD=your_secure_password
    volumes:
      - ./data:/var/lib/postgresql/data

  app:
    image: joplin/server:latest
    container_name: joplin
    depends_on:
      - db
    ports:
      - "127.0.0.1:22300:22300"
    restart: unless-stopped
    environment:
      - APP_BASE_URL=https://joplin.example.com
      - APP_PORT=22300
      - DB_CLIENT=pg
      - POSTGRES_HOST=db
      - POSTGRES_DATABASE=joplin
      - POSTGRES_USER=joplin
      - POSTGRES_PASSWORD=your_secure_password
      - POSTGRES_PORT=5432

```

### Login with
email: `admin@localhost`
password: `admin`

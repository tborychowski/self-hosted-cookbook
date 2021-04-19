# Vikujna

- very active development
- lots of features!
- UI looks better with every update 😄

<br>

- [Homepage](https://vikunja.io/)
- [Git repo](https://kolaente.dev/vikunja/)
- [Demo](https://try.vikunja.io/login) (demo:demo)


## docker-compose.yml
```yml
version: '3'
services:
  api:
    image: vikunja/api
    container_name: vikunja-api
    restart: unless-stopped
    environment:
      - VIKUNJA_SERVICE_TIMEZONE=Europe/Dublin
      - VIKUNJA_SERVICE_ENABLEREGISTRATION=false
      - VIKUNJA_SERVICE_JWTSECRET=ce23d1aezoosah2bao3ieZohkae5aicah
      - VIKUNJA_CACHE_ENABLED=true
      - VIKUNJA_CACHE_TYPE=memory
    volumes:
      - ./vikunja.db:/app/vikunja/vikunja.db
      - ./files:/app/vikunja/files

  frontend:
    image: vikunja/frontend
    container_name: vikunja-ui
    restart: unless-stopped

  proxy:
    image: nginx
    restart: unless-stopped
    ports:
      - 3111:80
    depends_on:
      - api
      - frontend
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
```

## nginx.conf
```nginx
server {
    listen 80;
    location / {
        proxy_pass http://frontend:80;
    }
    location ~* ^/(api|dav|\.well-known)/ {
        proxy_pass http://api:3456;
        client_max_body_size 20M;
    }
}
```


## Tips & Tricks
Before running, ake sure that db file exists first:
```sh
touch vikunja.db
```

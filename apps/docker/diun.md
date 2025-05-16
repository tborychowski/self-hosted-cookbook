# Diun
- Monitor docker images and check for updates.
- too many (ugly) notifications
- doesn't auto update, only monitors

<br>

- [Github repo](https://github.com/crazy-max/diun)
- [DockerHub repo](https://hub.docker.com/r/crazymax/diun/)
- [Docs](https://crazymax.dev/diun/)


## docker-compose.yml
```yml
version: "3.2"
services:
  diun:
    image: crazymax/diun:latest
    container_name: diun
    restart: unless-stopped
    environment:
      - "TZ=Europe/Dublin"
      - "LOG_LEVEL=info"
      - "LOG_JSON=false"
    volumes:
      - "./data:/data"
      - "./diun.yml:/diun.yml:ro"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

## diun.yml
```yml
db:
  path: diun.db

watch:
  workers: 10
  schedule: "0 * * * *"
  first_check_notif: true

notif:
  slack:
    webhook_url: https://hooks.slack.com/services/a/b/c

regopts:
  tom:
    username: admin
    password: Passw0rd!
    timeout: 20

providers:
  docker:
    watch_stopped: false
    watch_by_default: true
```

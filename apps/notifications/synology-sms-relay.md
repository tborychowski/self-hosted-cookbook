# Synology sms relay

- [Github repo](https://github.com/tborychowski/synology-sms-relay)
- [DockerHub repo](https://hub.docker.com/repository/docker/tborychowski/synology-sms-relay)
- [Docs](https://github.com/tborychowski/synology-sms-relay#docker-compose)


## docker-compose.yml
```yml
---
version: '3.7'
services:
  synology-sms-relay:
    container_name: synology-sms-relay
    image: tborychowski/synology-sms-relay
    restart: unless-stopped
    ports:
      - "3000:3000"
    volumes:
      - ./script.sh:/app/script.sh
```

## Example pushover `script.sh`
```sh
#!/bin/bash

# info    #666d7b
# success #408062
# warning #af8a1a
# danger  #8b4848

MSG=$(printf "<font color=\"#408062\">%s</font>" "$@")
curl -s -X POST \
    --data-urlencode "token=<app key>" \
    --data-urlencode "user=<user key>" \
    --data-urlencode "sound=pushover" \
    --data-urlencode "priority=0" \
    --data-urlencode "html=1" \
    --data-urlencode "title=Home Server" \
    --data-urlencode "message=$MSG" \
    https://api.pushover.net/1/messages.json
```

# Searx

- Simple & good looking UI
- Configurable
- Poor keyboard support

<br>

- [Homepage](https://searx.github.io/searx/)
- [Github repo](https://github.com/searx/searx)
- [Github repo for Docker](https://github.com/searx/searx-docker)

## Configuration
[settings.yml](searx-settings.yml)

## docker-compose.yml
```yml
version: '3.7'

services:

  filtron:
    container_name: filtron
    image: dalf/filtron
    restart: unless-stopped
    ports:
      - "4040:4040"
      - "4041:4041"
    networks:
      - searx
    command: -listen 0.0.0.0:4040 -api 0.0.0.0:4041 -target searx:8080
    volumes:
      - ./rules.json:/etc/filtron/rules.json:rw
    read_only: true
    cap_drop:
      - ALL

  searx:
    container_name: searx
    image: searx/searx:latest
    restart: unless-stopped
    networks:
      - searx
    command: ${SEARX_COMMAND:-}
    volumes:
      - ./searx:/etc/searx:rw
    environment:
      - BIND_ADDRESS=0.0.0.0:8080
      - BASE_URL=https://${SEARX_HOSTNAME:-localhost}/
      - MORTY_URL=https://morty.example.com/
      - MORTY_KEY=${MORTY_KEY}
    cap_drop:
      - ALL
    cap_add:
      - CHOWN
      - SETGID
      - SETUID
      - DAC_OVERRIDE

  morty:
    container_name: morty
    image: dalf/morty
    restart: unless-stopped
    ports:
      - "4045:3000"
    networks:
      - searx
    command: -timeout 6 -ipv6
    environment:
      - MORTY_KEY=${MORTY_KEY}
      - MORTY_ADDRESS=0.0.0.0:3000
    logging:
      driver: none
    read_only: true
    cap_drop:
      - ALL

  searx-checker:
    container_name: searx-checker
    image: searx/searx-checker
    restart: unless-stopped
    networks:
      - searx
    command: -cron -o html/data/status.json http://searx:8080
    volumes:
      - ./searx-checker:/usr/local/searx-checker/html/data:rw


networks:
  searx:
    ipam:
      driver: default
```

# Baserow

- Open source no-code database and [Airtable](https://airtable.com) alternative.

<br>

- [Homepage](https://baserow.io)


## docker-compose.yml
```yml
---
services:
  baserow:
    image: baserow/baserow:latest
    container_name: baserow
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
      - BASEROW_PUBLIC_URL=http://192.168.1.10:3123
    ports:
      - "3123:80"
    volumes:
      - ./data:/baserow/data
```

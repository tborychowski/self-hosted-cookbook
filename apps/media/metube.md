# Metube

- [Github repo](https://github.com/alexta69/metube)
-
![Screenshot](metube.png)

## docker-compose.yml

```yml
---
version: "3"
services:
  metube:
    image: alexta69/metube
    container_name: metube
    restart: unless-stopped
    user: "1001:1001"
    ports:
      - "8081:8081"
    volumes:
      - ./downloads:/downloads
```

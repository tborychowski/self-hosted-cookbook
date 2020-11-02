# Etesync

CalDAV & CardDav server & client.
Not terrible, but not great.

<br>

- [Homepage](https://www.etesync.com/)
- [Github repo](https://github.com/etesync)


## docker-compose.yml
```yml
---
version: '3'
services:
  etesync:
    image: dsaier/etesync
    container_name: etesync
    restart: unless-stopped
    environment:
      - TIME_ZONE=Europe/Dublin
      - AUTO_MIGRATE=true
      - SUPER_USER=admin
      - SUPER_PASS=admin
      - SUPER_EMAIL=admin@example.com
    ports:
      - "8001:8080"
    volumes:
      - ./data:/data
```
Then go to: http://localhost:8001/admin/logout/
and login with `admin`:`admin`

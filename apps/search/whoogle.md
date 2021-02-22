# Whoogle

- not the best looking
- keyboard support (thanks to [me](https://github.com/benbusby/whoogle-search/pull/202) :wink:)

<br>

- [Homepage](https://benbusby.com/projects/whoogle-search/)
- [Github repo](https://github.com/benbusby/whoogle-search)


## docker-compose.yml
```yml
---
version: "3"

services:
  whoogle-search:
    image: benbusby/whoogle-search
    container_name: whoogle-search
    restart: unless-stopped
    ports:
      - 3123:5000
```

# TubeArchivist

- [Homepage](https://www.tubearchivist.com)
- [Github repo](https://github.com/tubearchivist/tubearchivist)

![Screenshot](tubearchivist.png)


## Prerequisities

1. Elastic Search in Docker requires the kernel setting of the host machine vm.max_map_count to be set to at least 262144.
    ```sh
    # temporarily set the value run:
    sudo sysctl -w vm.max_map_count=262144
    ```
    To apply the change permanentlye.g. on Ubuntu Server add: `vm.max_map_count = 262144`
    to the file `/etc/sysctl.conf`.
    (more info in the [docs](https://github.com/tubearchivist/tubearchivist#vmmax_map_count)).

2. Elasticsearch has issues with permissions, so when using bind volume, folder has to be writable by the correct user. If you don't care, just run these:
    ```sh
    mkdir elasticsearch
    sudo chmod 777 elasticsearch
    ```

## docker-compose.yml
```yml
---
services:
  archivist-es:
    image: bbilly1/tubearchivist-es
    container_name: tubearchivist-es
    restart: unless-stopped
    ulimits:
      memlock:
        soft: -1
        hard: -1
    environment:
      - "ELASTIC_PASSWORD=verysecret"
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - "xpack.security.enabled=true"
      - "discovery.type=single-node"
      - "path.repo=/usr/share/elasticsearch/data/snapshot"
    expose:
      - "9200"
    volumes:
      - ./elasticsearch:/usr/share/elasticsearch/data


  archivist-redis:
    image: redislabs/rejson                 # for arm64 use bbilly1/rejson
    container_name: tubearchivist-redis
    restart: unless-stopped
    depends_on:
      - archivist-es
    expose:
      - "6379"
    volumes:
      - ./redis:/data


  tubearchivist:
    image: bbilly1/tubearchivist
    container_name: tubearchivist
    restart: unless-stopped
    depends_on:
      - archivist-es
      - archivist-redis
    environment:
      - HOST_UID=1000
      - HOST_GID=1000
      - TZ=Europe/Dublin
      - ES_URL=http://archivist-es:9200
      - ELASTIC_PASSWORD=verysecret
      - REDIS_HOST=archivist-redis
      - TA_HOST=192.168.1.123				# server ip, or domain
      - TA_USERNAME=admin                   # initial credentials
      - TA_PASSWORD=admin                   # initial credentials
    ports:
      - 3123:8000
    volumes:
      - ./media:/youtube
      - ./cache:/cache
```

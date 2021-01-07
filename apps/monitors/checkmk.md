# CheckMK

- Pretty complete solution for whole infrastructure monitoring
- Based on Nagios 

<br>

- [Homepage](https://checkmk.com/)
- [Github repo](https://github.com/tribe29/checkMK)
- [DockerHub repo](https://hub.docker.com/r/checkmk/check-mk-raw)


## docker-compose.yml
```yml
---
version: '3.6'

services:
  checkmk:
    container_name: checkmk
    image: checkmk/check-mk-raw
    tmpfs:
      - /opt/omd/sites/cmk/tmp:uid=1000,gid=1000
    ulimits:
      nofile: 1024
    volumes:
      - ./monitoring:/omd/sites
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "8080:5000"
    restart: unless-stopped
    networks:
      checkmk_network:

networks:
    checkmk_network:
```

- and go to http://localhost:8080/cmk/check_mk/
- You will find the provisional password for the cmkadmin account in the logs that are written for this container

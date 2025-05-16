# CheckMK

- Pretty complete solution for whole infrastructure monitoring
- Based on Nagios
- Complex UI (not very intuitive)
- Requires "some" learning & setup and doesn't do anything out of the box

<br>

- [Homepage](https://checkmk.com/)
- [Github repo](https://github.com/tribe29/checkMK)
- [DockerHub repo](https://hub.docker.com/r/checkmk/check-mk-raw)


## docker-compose.yml
```yml

services:
  checkmk:
    image: checkmk/check-mk-raw
    container_name: checkmk
    restart: unless-stopped
    ulimits:
      nofile: 1024
    ports:
      - "3123:5000"
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./monitoring:/omd/sites
```

- Open http://localhost:8080/cmk/check_mk/
- Username is `cmkadmin`
- Password is written in the logs when the container starts the first time, so just run `docker-compose logs` after starting the container

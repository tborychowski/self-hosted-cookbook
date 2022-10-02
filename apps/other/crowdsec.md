# Crowdsec

It's basically a self-hosted crowd-based firewall.
Setup is a bit cumbersome but (probably) well worth it :-)

<br>

- [Homepage](https://www.crowdsec.net)
- [Github repo](https://github.com/crowdsecurity/crowdsec)
- [Docker Hub](https://hub.docker.com/r/crowdsecurity/crowdsec)
- [Crowdsec Hub](https://hub.crowdsec.net)
- [Traefik bouncer](https://github.com/fbonalair/traefik-crowdsec-bouncer)
- [Collections](https://hub.crowdsec.net/browse/#collections)



## How does that work
- There are 2 parts of the solution: analyser & bouncer
- Crowdsec container (below) just basically analyses your server logs
- Bouncer container (below) uses the analysis to bounce off the potential attacks

## Local Setup
This describes how to setup crowdsec with traefik bouncer. There are other bouncers you can use (if you don't use traefik).

1. Create 2 files with the following content (`acquis.yml` and `docker-compose.yml`). Remember to update the paths to your logs in `docker-compose.yml`!
2. Start the containers (`docker compose up -d`)
3. Wait a minute or so (until it finishes installing collections), you can follow the logs to see what's going on (`docker compose logs -f`)
4. Add bouncer to the crowdsec instance:
    ```sh
    docker exec crowdsec cscli bouncers add traefik-bouncer
    ```
5. Copy the API key printed in the command output and paste it back in the `docker-compose.yml` in the bouncer config (`CROWDSEC_BOUNCER_API_KEY`)
6. Restart the containers
7. That's it.

## Online console
Unless you want to have an online console, than do this as well:
1. Register at https://app.crowdsec.net/
2. Enroll your instance, with the API key from there, e.g.:
    ```sh
    docker exec crowdsec cscli console enroll cl8m56qpu00060vlcwgj898z0
    ```

## Traefik middleware
1. Add traefik middleweare in the dynamic config, e.g.
    ```toml
    [http.middlewares.crowdsec.forwardauth]
    address = "http://<server ip>:3300/api/v1/forwardAuth"
    ```
2. Use this middleware in your services, e.g.
    ```toml
    [http.routers.authelia]
    rule ="Host(`login.domain.com`)"
    service = "authelia"
    tls = { }
    middlewares = [ "crowdsec" ]
    ```


## acquis.yml
```yml
---
filenames:
 - /logs/auth.log
 - /logs/syslog
 - /logs/kern.log
labels:
  type: syslog

---
filenames:
  - /logs/apache2/*.log
  - /logs/*httpd*.log
  - /logs/httpd/*log
labels:
  type: apache2

---
filenames:
  - /logs/nginx/*.log
labels:
  type: nginx

---
filenames:
 - /logs/authelia.log
labels:
  type: authelia

---
filenames:
  - /logs/traefik/*.log
labels:
  type: traefik
```

## docker-compose.yml
```yml
---
services:
  crowdsec:
    image: crowdsecurity/crowdsec
    container_name: crowdsec
    restart: unless-stopped
    environment:
      - GID="${GID-1000}"
      - COLLECTIONS=crowdsecurity/linux crowdsecurity/iptables crowdsecurity/apache2 crowdsecurity/sshd crowdsecurity/traefik LePresidente/authelia crowdsecurity/nginx crowdsecurity/base-http-scenarios
    volumes:
      - /var/log/auth.log:/logs/auth.log:ro
      - /var/log/syslog.log:/logs/syslog.log:ro
      - /var/log/kern.log:/logs/kern.log:ro
      - /var/log/apache:/logs/apache:ro
      - /var/log/httpd:/logs/httpd:ro
      - /var/log/authelia.log:/logs/authelia.log:ro
      - /var/log/traefik/logs:/logs/traefik:ro

      - ./acquis.yml:/etc/crowdsec/acquis.yaml
      - ./data:/var/lib/crowdsec/data/
      - ./config:/etc/crowdsec/

  bouncer:
    image: fbonalair/traefik-crowdsec-bouncer
    container_name: crowdsec-bouncer
    restart: unless-stopped
    environment:
      - PORT=8090
      - CROWDSEC_BOUNCER_API_KEY=changeme
      - CROWDSEC_AGENT_HOST=crowdsec:8080
    ports:
      - 3300:8090
```


## Useful commands

1. List installed items
```sh
docker exec crowdsec cscli scenarios list
docker exec crowdsec cscli collections list
docker exec crowdsec cscli parsers list
```

2. Block/unblock an ip
```sh
docker exec crowdsec cscli decisions add --ip 192.168.1.1
docker exec crowdsec cscli decisions remove --ip 192.168.1.1
docker exec crowdsec cscli decisions list
docker exec crowdsec cscli decisions help			# display help on decisions command
docker exec crowdsec cscli decisions add --help		# display help on add command
```

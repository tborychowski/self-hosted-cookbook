# Portainer
A nice UI for managing docker/kubernetes/swarm containers.

<br>

- [Homepage](https://www.portainer.io)
- [Github repo](https://github.com/portainer/portainer)


## docker-compose.yml
```yml
---
services:
  portainer:
    image: portainer/portainer-ce
    container_name: portainer
    restart: unless-stopped
    ports:
      - 8000:8000
      - 9443:9443
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./data:/data
```

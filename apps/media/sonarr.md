# Sonarr

TV show manager.

<br>

- [Homepage](https://sonarr.tv/)
- [Github repo](https://github.com/Sonarr/Sonarr)


## docker-compose.yml
```yml
services:
  sonarr:
    image: linuxserver/sonarr
    container_name: sonarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
      - UMASK_SET=022 #optional
    ports:
      - 8989:8989
    volumes:
      - ./config:/config
      - /mnt/video:/video
```


## Tips & Tricks

### Map remote folders on Ubuntu/Debian:
Add lines to the **/etc/fstab** file:

```sh
<SERVER IP>:/volume1/video /mnt/video nfs rw,hard,intr,nolock 0 0
```

### Remote Path Mappings in Sonarr/Radarr settings:

Make sure that in Download Client (advanced settings):

| Host        | Remote Path    | Local Path  |
|-------------|----------------|-------------|
| <SERVER IP> | /volume1/video | /downloads/ |

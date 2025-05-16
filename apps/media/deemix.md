# Deemix

- [Gitlab repo](https://gitlab.com/Bockiii/deemix-docker)


## docker-compose.yml
```yml
services:
  deemix:
    image: registry.gitlab.com/bockiii/deemix-docker
    container_name: Deemix
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
    ports:
      - 6595:6595
    volumes:
      - ./config:/config
      - ./downloads:/downloads
```


## Tips & Tricks
### [Getting ARL](https://codeberg.org/RemixDev/deemix/wiki/Getting-your-own-ARL)

- Go to [deezer.com](https://www.deezer.com) and log into your account
- Open up Developer Tools
- Go to Storage and open Cookies section
- Select www.deezer.com
- Find the arl cookie (It should be 192 chars long)
- Make sure only copy the value and not the entire cookie

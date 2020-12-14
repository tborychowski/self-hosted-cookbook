# Code Server

VSCode in the browser!

<br>

- [Github repo](https://github.com/cdr/code-server)
- [Docker Hub](https://hub.docker.com/r/linuxserver/code-server)


## docker-compose.yml
```yml
---
version: "2.1"
services:
  code-server:
    image: linuxserver/code-server
    container_name: code-server
    restart: unless-stopped
    ports:
      - 3000:8443
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
#      - PASSWORD=password #optional
#      - SUDO_PASSWORD=password #optional
	  - PROXY_DOMAIN=code.example.com #optional

	  # To get extensions from M$ store - add these:
      - SERVICE_URL=https://marketplace.visualstudio.com/_apis/public/gallery
      - ITEM_URL=https://marketplace.visualstudio.com/items
    volumes:
      - ./config:/config
	  # folder that will show up in the Code file tree
	  - /var/www/project1:/config/workspace/project1
```

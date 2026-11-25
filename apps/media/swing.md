# Swing Music

- Very simple setup and JustWorks®
- Not much options to set
- Fetches metadata from external services
- iOS app is comint (TestFlight as of Nov 2026)

<br>

- [Homepage](https://swingmx.com/)
- [Github repo](https://github.com/swingmx/swingmusic/)
- [Docs](https://swingmx.com/guide/introduction.html)


## docker-compose.yml

```yml
services:
  swingmusic:
    image: ghcr.io/swingmx/swingmusic:latest
    container_name: swingmusic
    restart: unless-stopped
    environment:
      - SWINGMUSIC_DEVICE_NAME=MyServer
      - SWINGMUSIC_PORT=1970
    ports:
      - "1970:1970"
    volumes:
      - ./config:/config
      - ./music:/music
```

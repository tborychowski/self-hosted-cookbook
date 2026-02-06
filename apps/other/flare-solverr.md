# FlareSolverr

Proxy server to bypass Cloudflare protection. Can be used by other apps (e.g. Jackett) to bypass Cloudflare's human verification.

<br>

- [Github repo](https://github.com/FlareSolverr/FlareSolverr)


## docker-compose.yml
```yml
  flaresolverr:
    image: ghcr.io/flaresolverr/flaresolverr:latest
    container_name: flaresolverr
    restart: unless-stopped
    ports:
      - 8191:8191
```

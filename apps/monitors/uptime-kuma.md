# Uptime Kuma
It is a self-hosted monitoring tool like "Uptime Robot".

- One of the best monitoring services that I've tested!
- Simple yet configurable, and a very nice design overall
- Fast and snappy!
- Great status page, that's available without a login
- Large number of options, notification services, etc.
- Supports 2fa or disabling authentication (if you wish to run it locally)

<br>

- [Github repo](https://github.com/louislam/uptime-kuma)
- [Demo](https://demo.uptime.kuma.pet/)
- [Tutorial blog post](https://homegrowntechie.com/uptime-kuma/)

## docker-compose.yml
```yml
version: '3.3'
services:
  uptime-kuma:
    image: louislam/uptime-kuma
    container_name: uptimekuma
    restart: unless-stopped
    volumes:
      - ./data:/app/data
    ports:
      - 3001:3001
```
First launch in browser will bring up a setup wizard.

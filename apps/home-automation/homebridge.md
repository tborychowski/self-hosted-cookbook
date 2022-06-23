# Homebridge

Homebridge allows you to integrate with smart home devices that do not natively support HomeKit.
Some of the most popular plugins include:

- Ring
- Nest & Nest Cameras
- TP-Link Kasa Smart Home
- Hue / deCONZ (Zigbee)
- Belkin Wemo
- myQ
- UniFi Protect

---

- [Homepage](https://homebridge.io/)
- [Github repo](https://github.com/homebridge/homebridge)
- [Docs](https://github.com/homebridge/homebridge/wiki)


## docker-compose.yml
```yml
---
services:
  homebridge:
    image: oznu/homebridge:latest
    container_name: homebridge
    restart: unless-stopped
    network_mode: host
    environment:
      - TZ=Europe/Dublin
      - PGID=1000
      - PUID=1000
      - HOMEBRIDGE_CONFIG_UI=1
      - HOMEBRIDGE_CONFIG_UI_PORT=8581
    volumes:
      - ./homebridge:/homebridge
```

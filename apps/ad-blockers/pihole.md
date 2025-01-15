# PiHole

## Overview
- [Homepage](https://pi-hole.net/)
- [Pihole Docker github repo](https://github.com/pi-hole/docker-pi-hole)
- [Docs](https://docs.pi-hole.net/)
- [Github repo](https://github.com/pi-hole)
- [DockerHub repo](https://hub.docker.com/r/pihole/pihole)


## `docker-compose.yml`
```yml
---
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
      - "443:443/tcp"
    #   - "67:67/udp"
    environment:
      TZ: 'Europe/Dublin'
      WEBPASSWORD: 'set a secure password here or it will be random'
    volumes:
      - ./etc-pihole/:/etc/pihole/
      - ./etc-dnsmasq.d/:/etc/dnsmasq.d/
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

## Tips & Tricks
Some information here may be outdated!

### Force DNS to PiHole with Unifi Secure Gateway
- https://help.ubnt.com/hc/en-us/articles/215458888-UniFi-USG-Advanced-Configuration

### Pihole block youtube ads
https://www.reddit.com/r/pihole/comments/84luw8/blocking_youtube_ads/dvqq6ax/

### DNS over HTTPS
https://docs.pi-hole.net/guides/dns-over-https/

### PiHole Conditional forwarding for multiple VLANs
If you have multiple VLANs on your router, then you might want conditional forwarding of all your subnets back to your router.

1. Create a new file:
   ```sh
   sudo nano /etc/dnsmasq.d/02-custom.conf
   ```
2. Then add, e.g.:
   ```sh
   server=/5.168.192.in-addr.arpa/192.168.1.1
   server=/9.168.192.in-addr.arpa/192.168.1.1
   ```
   Which covers `192.168.5.0/24` and `192.168.9.0/24` respectively.
3. Restart PiHole:
   ```sh
   pihole restartdns
   ```

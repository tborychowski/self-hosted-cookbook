# Gluetun VPN client
A VPN client to tunnel to Cyberghost, ExpressVPN, FastestVPN, HideMyAss, IPVanish, IVPN, Mullvad, NordVPN, Perfect Privacy, Privado, Private Internet Access, PrivateVPN, ProtonVPN, PureVPN, Surfshark, TorGuard, VPNUnlimited, VyprVPN, WeVPN and Windscribe VPN servers using Go, OpenVPN or Wireguard, iptables, DNS over TLS, ShadowSocks and an HTTP proxy.<br>

- [Github repo](https://github.com/qdm12/gluetun)
- [Docker Hub](https://hub.docker.com/r/qmcgaw/gluetun)

## Requirements
An account with a compatible VPN provider is required.

## docker-compose.yml
```yml
---
services:
  gluetun:
    image: qmcgaw/gluetun
    container_name: gluetun
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
    environment:
      - TZ=Europe/Dublin
      - VPN_TYPE=openvpn
      - VPNSP=fastestvpn
      - OPENVPN_USER=<VPN LOGIN>
      - OPENVPN_PASSWORD=<VPN PASSWORD>
      - COUNTRY=<VPN COUNTRY>
    volumes:
      - ./data:/gluetun
      - ./data/port_forward:/tmp/gluetun/forwarded_port
    ports:
      #- 8888:8888/tcp # HTTP proxy
      - 3020:8000/tcp # Built-in HTTP control server
      - 9117:9117     # jackett
      - 6881:6881     # qbit
      - 3030:3030     # qbit webUI
```

and then - in the corresponding service `docker-compose.yml`, e.g.:
```yml
---
services:
  jackett:
    image: ghcr.io/linuxserver/jackett
    network_mode: "container:gluetun"
```

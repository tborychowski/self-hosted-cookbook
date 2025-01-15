# Tautulli

Monitor for Plex Media Server.

<br>

- [Homepage](https://tautulli.com/)
- [Github repo](https://github.com/Tautulli/Tautulli)
- [Docs](https://github.com/Tautulli/Tautulli-Wiki/wiki/Installation)



## docker-compose.yml
```yml
---
services:
  tautuli:
    image: tautulli/tautulli
    container_name: tautuli
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
    ports:
      - "8181:8181"
    volumes:
      - ./config:/config/
      - /mnt/plex:/plex_logs:ro
```


## Tips & Tricks

### Mount external server with Plex (e.g. synology NAS)

1. Edit `fstab` file:
    `sudo nano /etc/fstab`
2. Add line
```sh
<SERVER IP>:/volume1/Plex/Library/Application\040Support/Plex\040Media\040Server/Logs /mnt/plex nfs ro,hard,intr,nolock 0 0
```

### Troubleshooting

#### libcgroup1
This lib may need to be installed:
```sh
apt-get install libcgroup1
```

#### Mountpoint for devices not found
1. Edit grub
    ```sh
    sudo nano /etc/default/grub
    ```
2. Change `GRUB_CMDLINE_LINUX` value to the below:
    ```sh
    GRUB_CMDLINE_LINUX="systemd.unified_cgroup_hierarchy=0"
    ```
3. Then run:
    ```sh
    grub2-mkconfig
    sudo reboot
    ```

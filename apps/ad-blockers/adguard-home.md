# Adguard Home

## Overview

- [Homepage](https://adguard.com/en/adguard-home/overview.html)
- [Overview blog post](https://adguard.com/en/blog/in-depth-review-adguard-home.html)
- [Github](https://github.com/AdguardTeam/AdguardHome)
- [DockerHub](https://hub.docker.com/r/adguard/adguardhome)

## `docker-compose.yml`
```yml
---
version: '3.7'
services:
  adguard:
    container_name: adguard
    image: adguard/adguardhome
    restart: unless-stopped
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "853:853/tcp"
      - "9001:80/tcp"
      - "9002:443/tcp"
      - "9003:3000/tcp"
    #  not sure what this is for,but everything works without it
    #  - "67:67/udp"
    #  - "68:68/tcp"
    #  - "68:68/udp"
    volumes:
      - ./conf:/opt/adguardhome/conf
      - ./data:/opt/adguardhome/work
      - ./certs:/opt/adguardhome/certs  # remember to put this path in the UI too
```
Admin panel should be available at https://localhost:9002


## Tips & tricks

### Reset UI password
1. Generate hash
    ```sh
    htpasswd -B -n -b <USERNAME> <PASSWORD>
    ```
    It will print `<USERNAME>:<HASH>` to the terminal.
2. Open AdGuardHome.yaml
    ```sh
    nano adguard/conf/AdGuardHome.yaml
    ```
3. In the `users:` section update:
   ```yml
   users:
    - name: username
      password: <HASH>
   ```
4. Save & restart AdguardHome


### How to map IP -> Hostname
Add `extra_hosts` node to the `docker-compose.yml` with the mapping, like so:
```yml
extra_hosts:
  - "HomeServer:192.168.1.20"
```
This will inject the map to the `/etc/hosts` file of the container,
which **AdGuard** will use to show the nice names.

Alternatively this can be done through the Admin UI.


###  Ensure that port 53 is available for the container
1. First, disable and stop Ubuntu's DNS resolver using the following two commands:
    ```sh
    sudo systemctl disable systemd-resolved.service
    sudo systemctl stop systemd-resolved.service
    ```
2. Next, open network manager configuration using the following command for editing:
    ```sh
    sudo nano /etc/NetworkManager/NetworkManager.conf
    ```
3. Add `dns=default` under `[main]` so that the file contents look like this:
    ```ini
    [main]
    plugins=ifupdown,keyfile
    dns=default
    ```
4. Then either remove or rename `/etc/resolv.conf` (it's a symbolic link) using the the following command:
    ```sh
    sudo mv /etc/resolv.conf /etc/resolv.conf.bak
    ```
5. In case there is a problem, you can rename the file back to `/etc/resolv.conf`.
6. Finally, restart your network manager using the following command:
    ```sh
    sudo service network-manager restart
    ```
7. If that doesn't work, use this:
    ```sh
    ps aux | grep dns   # get the PID of the dns process
    sudo kill -9 <PID>  # kill it
    ```

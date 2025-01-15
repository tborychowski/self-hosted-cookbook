# Windows 7 in a Docker container

- [Github repo](https://github.com/dockur/windows)


## docker-compose.yml
```yml
---
services:
  windows:
    image: dockurr/windows
    container_name: windows
    restart: unless-stopped
    devices:
      - /dev/kvm
      - /dev/net/tun
    cap_add:
      - NET_ADMIN
    ports:
      - 8006:8006
      - 3389:3389/tcp
      - 3389:3389/udp
    stop_grace_period: 2m
    environment:
      CPU_CORES: "4"
      RAM_SIZE: "8G"
      DISK_SIZE: "128G"
      USERNAME: "admin"
    volumes:
       - ./windows:/storage
       - ./win7.iso:/custom.iso
```

## Tips & Tricks

### Custom ISO image
Just add this bit (and remove the `VERSION` environment variable) to the `docker-compose.yml` file:

```yml
  volumes:
    - /home/user/example.iso:/custom.iso
```

### CPU & RAM
By default, the container will be allowed to use a maximum of 2 CPU cores and 4 GB of RAM.
If you want to adjust this, you can specify the desired amount using the following environment variables:

```yml
  environment:
    RAM_SIZE: "8G"
    CPU_CORES: "4"
```

### Username and password
By default, a user called `Docker` is created during the installation, with an **empty password**.
If you want to use different credentials, you can change them in your compose file:

```yml
  environment:
    USERNAME: "bill"
    PASSWORD: "gates"
```

### Storage location
To change the storage location, include the following bind mount in your compose file:

```yml
  volumes:
    - /var/win:/storage
```


### Storage size
To expand the default size of **64 GB**, add the `DISK_SIZE` setting to your compose file and set it to your preferred capacity:

```yml
  environment:
    DISK_SIZE: "256G"
```

### Sharing files with the host
Open **File Explorer** and click on the **Network** section, you will see a computer called **host.lan**. Double-click it and it will show a folder called **Data**, which can be bound to any folder on your host via the compose file:

```yml
  volumes:
    - /home/user/example:/data
```

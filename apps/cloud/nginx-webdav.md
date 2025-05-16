## docker-nginx-webdav-nononsense

aims to be a Docker image that enables a no-nonsense WebDAV system on the latest available nginx, stable and mainline. Serves a file server

<br>

- [Github repo](https://github.com/dgraziotin/docker-nginx-webdav-nononsense/)

## docker-compose-yml

```yml
services:
    nginxwebdav:
        container_name: nginxwebdav
        build:
            context: .
        volumes:
            - ./data:/data
            - ./config:/config
        environment:
            - PUID=501
            - PGID=20
            - TZ=Europe/Berlin
            - WEBDAV_USERNAME=user
            - WEBDAV_PASSWORD=password
            - SERVER_NAMES=localhost
            - TIMEOUTS_S=1200 # these are seconds
            - CLIENT_MAX_BODY_SIZE=120M # must end with M(egabytes) or G(igabytes)
        ports:
            - 32080:80
```

## Tips & Tricks

### customise nginx.conf

Change dav_access user:rw group:rw all:rw to "location" context to custoise client file permissions

and change dav_ethods to PUT, DELETE, MKCOL, COPY, and MOVE so that clients can erform any action

alternatively you can create a custom config under /config/custom-cont-init.d/

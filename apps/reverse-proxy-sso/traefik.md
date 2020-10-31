# Traefik
This is one of the best reverse-proxy solutions for self-hosting.
Very easy to run & maintain (once you pass the setup).<br>

Traefik can detect docker services and use docker labels to automatically create routes.
However, I prefer to keep my docker-compose files clean and explicitly set routers & services myself, so this solution does that exactly.<br>

Traefik can also be set-up to automatically provide Let's Encrypt certs for your services.
However, there are some services that need cert files (AdGuard Home, Mailcow), and because I want to have a single wildcard certificate for my whole domain (and all subdomains) I prefer to generate it manually (i.e. scripts in cron) and just reference it whenever it's required - so this setup reflects that.

## General overview
Traefik has 2 types of config: static (requires restart of the container) and dynamic (refreshes live).
Dynamic config can be provided as a folder, where all `yml` files are parsed and configuration from them is applied to the running server.
You can create multiple files and split the dynamic config to your preference. I prefer to keep the 2 main layers (routers & services) separate, as it's easy for me to structure the files and it's clear to see what services are defined and the ports that they use. The down-side is that adding/removing a service requires editing 2 files.<br>
Another approach would be to use 1 yaml file per service (with route & service definition). It would be clearer from the Filesystem (ls -al) to see what services are configured, but e.g. checking all ports would require viewing all config files.<br>
For that reason it's also good to keep a note somewhere with a table of service-port mapping.

<br>

- [Homepage](https://traefik.io/)
- [Github repo](https://github.com/traefik)
- [Docs](https://doc.traefik.io/traefik/)

## docker-compose.yml
```yml
version: '3'
services:
  traefik:
    image: traefik:v2.3
    container_name: traefik
    restart: unless-stopped
    security_opt: ["no-new-privileges"]
    ports:
      - "80:80"
      - "443:443"
      - "3080:8080"
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /path/to/certs:/certs:ro
      - ./config:/config:ro
      - ./traefik.yml:/traefik.yml:ro
```

## Static config

### traefik.yml
```yml
global:
  checkNewVersion: true
  sendAnonymousUsage: false

api:
  dashboard: true
  insecure: true

entryPoints:
  http:
    address: ":80"
  https:
    address: ":443"

serversTransport:
  insecureSkipVerify: true

providers:
  file:
    directory: /config
    watch: true
```

## Dynamic config

### config/middlewares.yml
```yml
http:
  middlewares:
    authelia:
      forwardAuth:
        address: http://<SERVER IP>:9091/api/verify?rd=https://login.example.com/
        trustForwardHeader: true

    redirect-to-https:
      redirectScheme:
        scheme: https
        permanent: true

    security-headers:
      headers:
        referrerPolicy: "same-origin"
        contentTypeNosniff: true
        frameDeny: false
        forceSTSHeader: true
        stsIncludeSubdomains: true
        stsPreload: true
        stsSeconds: 15552000

    nextcloud-redirectregex:
      redirectRegex:
        permanent: true
        regex: 'https://(.*)/.well-known/(card|cal)dav'
        replacement: 'https://${1}/remote.php/dav/'

    some-redirect:
      redirectRegex:
        regex: "https://subdomain1.example.com/"
        replacement: "https://subdomain2.example.com?query=123"
        permanent: true

```

### config/tls.yml
```yml
tls:
  certificates:
    - certFile: /example1-com/fullchain.cer
      keyFile: /example1-com/example1.com.key
      stores:
        - default
    - certFile: /example2-com/fullchain.cer
      keyFile: /example2-com/example2.com.key
      stores:
        - default

  stores:
    default:
      defaultCertificate:
        certFile: /example1-com/fullchain.cer
        keyFile: /example1-com/example1.com.key
```

### config/routers.yml
```yml
http:
  routers:
    authelia:
      rule: "Host(`login.example.com`)"
      service: authelia
      tls: {}
      middlewares:
        - security-headers

    nextcloud:
      rule: "Host(`cloud.example.com`)"
      service: nextcloud
      tls: {}
      middlewares:
        - security-headers
        - nextcloud-redirectregex

    sonarr:
      rule: "Host(`sonarr.example.com`)"
      service: sonarr
      tls: {}
      middlewares:
        - security-headers
        - authelia
```


### config/services.yml
```yml
http:
  services:

    authelia:
      loadBalancer:
        servers:
          - url: "http://<SERVER IP>:9091"

    nextcloud:
      loadBalancer:
        servers:
          - url: "http://<SERVER IP>:3100"

    sonarr:
      loadBalancer:
        servers:
          - url: "http://<SERVER IP>:8989/"
```

## Useful links
- [Traefik 2 + Docker — a Simple Step by Step Guide](https://medium.com/@containeroo/traefik-2-0-docker-a-simple-step-by-step-guide-e0be0c17cfa5#37d9)
- [Traefik 2 + Docker — an Advanced Guide](https://medium.com/@containeroo/traefik-2-0-docker-an-advanced-guide-d098b9e9be96)
- [Traefik 2 & TLS 101](https://containo.us/blog/traefik-2-tls-101-23b4fbee81f1/)
- [check security headers](https://securityheaders.com)

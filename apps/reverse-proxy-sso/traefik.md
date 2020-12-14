# Traefik
This is one of the best reverse-proxy solutions for self-hosting.
Very easy to run & maintain (once you pass the setup).<br>

Traefik can detect docker services and use docker labels to automatically create routes.
However, I prefer to keep my docker-compose files clean and explicitly set routers & services myself, so this solution does exactly that.<br>

Traefik can also be set-up to automatically provide Let's Encrypt certs for your services.
However, there are some services that need cert files (AdGuard Home, Mailcow), and because I want to have a single wildcard certificate for my whole domain (and all subdomains) I prefer to generate it manually (i.e. scripts in cron) and just reference it whenever it's required - so this setup reflects that.

## General overview
Traefik has 2 types of config:
- static - requires restart of the container
- dynamic - refreshes live.

Dynamic config can be provided as a folder, where all `toml`/`yml` files are parsed and configuration from them is applied to the running server.<br>

You can create multiple files and split the dynamic config to your preference. I prefer to keep 1 main file (for tls/cert settings and global middlewares), and then add 1 config file per service (with route & service definition).<br>

It's also good to keep a note with a table of service-port mapping (to quickly see which ports are used by which service).

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

### config/_main.toml
```toml
[[tls.certificates]]
certFile = "/certs-com/fullchain.cer"
keyFile = "/certs-com/example.com.key"
stores = [ "default" ]

[[tls.certificates]]
certFile = "/certs-org/fullchain.cer"
keyFile = "/certs-org/example.org.key"
stores = [ "default" ]

[tls.stores.default.defaultCertificate]
certFile = "/certs-com/fullchain.cer"
keyFile = "/certs-com/example.com.key"

[http.middlewares.redirect-to-https.redirectScheme]
scheme = "https"
permanent = true

[http.middlewares.security-headers.headers]
referrerPolicy = "same-origin"
contentTypeNosniff = true
frameDeny = false
forceSTSHeader = true
stsIncludeSubdomains = true
stsPreload = true
stsSeconds = 15_552_000

[[http.services.noop.loadBalancer.servers]]
url = "http://192.168.0.1:666"  # this is a fake url

[http.routers.http-catchall]
rule = "HostRegexp(`{host:(www\\.)?.+}`)"
entryPoints = [ "http" ]
middlewares = [ "redirect-to-https" ]
service = "noop"
```


### config/service-authelia.toml
```toml
[http.middlewares.authelia.forwardAuth]
address = "http://<SERVER IP>:9091/api/verify?rd=https://login.example.com/"
trustForwardHeader = true

[[http.services.authelia.loadBalancer.servers]]
url = "http://<SERVER IP>:9091"

[http.routers.authelia]
rule ="Host(`login.example.com`)"
service = "authelia"
tls = { }
middlewares = [ "security-headers" ]
```

### config/service-nextcloud.toml
```toml
[http.middlewares.nextcloud-redirectregex.redirectRegex]
permanent = true
regex = "https://(.*)/.well-known/(card|cal)dav"
replacement = "https://${1}/remote.php/dav/"

[[http.services.nextcloud.loadBalancer.servers]]
url = "http://<SERVER IP:3000"

[http.routers.nextcloud]
rule = "Host(`cloud.example.com`)"
service = "nextcloud"
tls = { }
middlewares = [ "security-headers", "nextcloud-redirectregex" ]
```

### config/service-sonarr.toml
```toml
[[http.services.sonarr.loadBalancer.servers]]
url = "http://<SERVER IP>:8989/"

[http.routers.sonarr]
rule = "Host(`sonarr.example.com`)"
service = "sonarr"
tls = { }
middlewares = [ "security-headers", "authelia" ]
```

### config/service-traefik.toml
```toml
[[http.services.traefik.loadBalancer.servers]]
url = "http://<SERVER IP>:3080"

[http.routers.traefik]
rule = "Host(`traefik.example.com`) && PathPrefix(`/dashboard`)"
service = "traefik"
tls = { }
middlewares = [ "security-headers", "authelia" ]

[http.routers.traefik_api]
rule = "Host(`traefik.example.com`)"
service = "api@internal"
tls = { }
middlewares = [ "security-headers", "authelia" ]
```



## Useful links
- [Traefik 2 + Docker — a Simple Step by Step Guide](https://medium.com/@containeroo/traefik-2-0-docker-a-simple-step-by-step-guide-e0be0c17cfa5#37d9)
- [Traefik 2 + Docker — an Advanced Guide](https://medium.com/@containeroo/traefik-2-0-docker-an-advanced-guide-d098b9e9be96)
- [Traefik 2 & TLS 101](https://containo.us/blog/traefik-2-tls-101-23b4fbee81f1/)
- [check security headers](https://securityheaders.com)

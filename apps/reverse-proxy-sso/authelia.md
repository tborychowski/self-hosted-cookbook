# Authelia
This is a fantastic, feature rich and simple to set-up Single Sign-On solution.
The config files below, will use a file-storage for users, because it's simpler and quite sufficient for simple self-hosting server at home (as opposed to seting up full featured LDAP back-end).

<br>

- [Homepage](https://www.authelia.com/)
- [Github repo](https://github.com/authelia/authelia)
- [Docs](https://www.authelia.com/docs/)


## docker-compose.yml
```yml
networks:
  net:
    driver: bridge

services:
  authelia:
    image: authelia/authelia
    container_name: authelia
    restart: unless-stopped
    expose:
      - 9091
    ports:
      - "9091:9091"
    networks:
      - net
    environment:
      - TZ=Europe/Dublin
    volumes:
      - ./config:/config

  redis:
    image: redis:alpine
    container_name: redis
    volumes:
      - ./redis:/data
    expose:
      - 6379
    networks:
      - net
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
```

## config/configuration.yml
```yml
server:
  host: 0.0.0.0
  port: 9091

# log:
#   level: debug

jwt_secret: U8kmbel7WhP1YneQh2134DXhsiSHctE5Emtf

authentication_backend:
  file:
    path: /etc/authelia/users.yml

storage:
  encryption_key: U8kmbel7WhP1YneQh2134DXhsiSHctE5Emtf
  local:
    path: /var/lib/authelia/db.sqlite3

notifier:
  filesystem:
    filename: /tmp/authelia/notification.txt

session:
  name: authelia_session
  secret: U8kmbel7WhP1YneQh2134DXhsiSHctE5Emtf
  expiration: 3600 # 1 hour
  inactivity: 300 # 5 minutes
  # The domain to protect.
  # Note: the login portal must also be a subdomain of that domain.
  domain: example.com
  redis:
    host: redis
    port: 6379

regulation:
  max_retries: 3
  find_time: 120
  ban_time: 300

access_control:
  default_policy: one_factor
  rules:
    - domain: "*.example.com"
      subject: "group:admins"
      policy: one_factor
```

## config/users.yml
```yml
users:
  admin:
    displayname: "admin"
    password: ""   # password hash - see below how to generate
    email: admin@example.com
    groups:
      - admins
```

## Tips & Tricks
Generate password hash for the `users.yml`:
```sh
docker run authelia/authelia:latest authelia hash-password <PASS>
```

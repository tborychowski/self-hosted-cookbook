# docker-radicale

Radicale is a small but powerful CalDAV (calendars, to-do lists) and CardDAV (contacts) server

<br>

- [Github repo](https://github.com/tomsquest/docker-radicale)

- [configuration file](https://github.com/tomsquest/docker-radicale/blob/master/config)


```yml
services:
  radicale:
    image: tomsquest/docker-radicale
    container_name: radicale
    ports:
      - 5232:5232
    init: true
    read_only: true
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: curl -f http://127.0.0.1:5232 || exit 1
      interval: 30s
      retries: 3
....restart: unless-stopped
    volumes:
      - ./data:/data
      - ./config:/config:ro
      - ./users:/etc/radicale/users
```

## Tips & tricks

### configuration file

create a config directory in the working dir

`mkdir -p config`

copy the configuration file from the link above into the config folder 


### Basic auth

Uncomment/Enable the following in the configuration file

 ```
[auth]
type = htpasswd
htpasswd_filename = /etc/radicale/users
htpasswd_encryption = md5
```

### htpasswd

flat-file used to store usernames and password for basic authentication 

While in the working dir

```
htpasswd -c users username
New password:
Re-type new password:
```

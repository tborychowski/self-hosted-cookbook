# Statping

- Looks great
- Has a mobile app (albeit buggy)
- Lots of stuff (charts, data, etc)
- Lots of integrations & notification types
- Every other release doesn't work
- Slow UI

<br>

- [Homepage](https://statping.com/)
- [Github repo](https://github.com/statping/statping/)
- [DockerHub repo](https://hub.docker.com/r/statping/statping)


## docker-compose.yml
```yml
---
statping:
  container_name: statping
  image: hunterlong/statping
  restart: unless-stopped
  ports:
    - 3020:8080
  volumes:
    - ./data:/app
#    - /var/run/docker.sock:/var/run/docker.sock
  environment:
    DB_CONN: sqlite
```

- and go to http://192.168.1.10:3020/setup
- or login as `admin:admin`

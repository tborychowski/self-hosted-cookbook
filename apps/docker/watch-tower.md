# WatchTower

- too many notifications
- some false positive (thanks to crappy docker-hub api)

<br>

- [Github repo](https://github.com/containrrr/watchtower/)
- [Docs](https://containrrr.dev/watchtower/)


## docker-compose.yml
```yml
services:
  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: --interval 30
    environment:
      - TZ=Europe/Dublin
      - WATCHTOWER_MONITOR_ONLY=true
      - WATCHTOWER_NOTIFICATIONS=slack
      - WATCHTOWER_NOTIFICATION_SLACK_HOOK_URL=https://hooks.slack.com/services/a/b/c
```

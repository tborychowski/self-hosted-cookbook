# NTFY
A self-hosted notification server (like pushover).

- has mobile apps for ios and android
- interesting conceptually (simple pub-sub)
- very easy to use (from curl to php)
- notifications arrive promptly, within seconds (1 - 10) (not as instant as e.g. pushover)
- sometimes apps need to be restarted to show notifications (sometimes a notification shows up in notification center, but not in the app)
- no sync between clients - i.e. the same notification must be marked as read or dismissed in all subscribers (in pushover, when I read one on my phone - it shows as read on laptop)

<br>

- [Homepage](https://ntfy.sh)
- [Github repo](https://github.com/binwiederhier/ntfy)
- [Docs](https://ntfy.sh/docs/)


## ntfy/server.yml
```yml
# options: https://ntfy.sh/docs/config/

base-url: https://ntfy.domain.com

# needed for performance
cache-file: /var/cache/ntfy/cache.db
cache-duration: "12h"
cache-startup-queries: |
    pragma journal_mode = WAL;
    pragma synchronous = normal;
    pragma temp_store = memory;

# This is needed for instant mobile notifications
upstream-base-url: "https://ntfy.sh"
```


## docker-compose.yml
```yml
---
services:
  ntfy:
    image: binwiederhier/ntfy
    container_name: ntfy
    restart: unless-stopped
    command:
      - serve
    environment:
      - TZ=Europe/Dublin
    volumes:
      - ./cache:/var/cache/ntfy
      - ./ntfy:/etc/ntfy
    ports:
      - 3040:80
```

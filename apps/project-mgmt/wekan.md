# Wekan

Probably the best of the ones tested. A good balance between features & complexity and a nice UI.

<br>

- [Homepage](https://wekan.github.io/)
- [Github repo](https://github.com/wekan/wekan)
- [Docs](https://github.com/wekan/wekan/wiki)

## docker-compose.yml
```yml
---
services:
  wekandb:
    image: mongo:latest
    container_name: wekan-db
    restart: unless-stopped
    command: mongod --oplogSize 128
    expose:
      - 27017
    volumes:
      - ./db:/data/db
      - ./db-dump:/dump

  wekan:
    image: wekanteam/wekan
    container_name: wekan-app
    restart: unless-stopped
    ports:
      - 3090:8080
    environment:
      - MONGO_URL=mongodb://wekandb:27017/wekan
      - ROOT_URL=http://localhost:3090
      - WITH_API=true # Wekan Export Board works when WITH_API=true.
      - RICHER_CARD_COMMENT_EDITOR=false
      - CARD_OPENED_WEBHOOK_ENABLED=false
      - BIGEVENTS_PATTERN=NONE
      - BROWSER_POLICY_ENABLED=true
    depends_on:
      - wekandb
```

## Tips & Tricks
- Creating users and logging-in: https://github.com/wekan/wekan/wiki/Adding-users
- Forgot password: https://github.com/wekan/wekan/wiki/Forgot-Password
- Backup/Restore: https://github.com/wekan/wekan/wiki/Backup

# Monocker

Monocker monitors Docker (MONitors dOCKER) containers and alerts on 'state' change.
There is no web ui or fancy dashboard.

<br>

- [Github repo](https://github.com/petersem/monocker)


## docker-compose

```yml
---
services:
  monocker:
    container_name: monocker
    image: petersem/monocker
    environment:
      SERVER_LABEL: 'Dev'
      MESSAGE_PLATFORM: 'telegram@your_bot_id@your_chat_id'
      # MESSAGE_PLATFORM: 'pushbullet@your_api_key@your_device_id'
      # MESSAGE_PLATFORM: 'pushover@your_user_key@your_app_api_token'
      # MESSAGE_PLATFORM: 'discord@webhook_url'
      LABEL_ENABLE: 'false'
      ONLY_OFFLINE_STATES: 'false''
      EXCLUDE_EXITED: 'false'. 
      PERIOD: 60
      DISABLE_STARTUP_MSG: 'false'
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    restart: unless-stopped
```

## Tips & tricks

### raspberry pi and arm users

There is only amd64 image available,  you have to build.

`get clone https://github.com/petersem/monocker`


Change `FROM node:14.17.3-alpine3.14` to `FROM node:latest` in the Dockerfile

And  `docker build -t monocker .`

Also change `image: petersem/monocker` to `image: monocker` in the docker-compose.yml

### telegram notifications

Contact @botfather,  create a bot and copy its token then start a chat with the bot and use the inline command @get_id_bot and copy the chat id

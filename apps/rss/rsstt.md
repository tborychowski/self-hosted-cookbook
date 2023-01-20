# RSS to telegram bot

A Telegram RSS bot that cares about your reading experience

- [GitHub repo](https://github.com/Rongronggg9/RSS-to-Telegram-Bot)
- [Docs](https://github.com/Rongronggg9/RSS-to-Telegram-Bot/tree/dev/docs)
- [env vars description](https://github.com/Rongronggg9/RSS-to-Telegram-Bot/blob/dev/docs/advanced-settings.md)

<br>

# docker-compose.yml

```yml
---
version: '3.6'

services:
  main:
    image: rongronggg9/rss-to-telegram:dev  # stable image: rongronggg9/rss-to-telegram
    container_name: rsstt  # need to be unique
    restart: unless-stopped
    volumes:
      - ./config:/app/config
    environment:
      - TOKEN=1234567890:A1BCd2EF3gH45IJK6lMN7oPqr8ST9UvWX0Yz0  # get it from @BotFather
      - MANAGER=1234567890  # get it from @userinfobot
      - TELEGRAPH_TOKEN=1a23b456c78de90f1a23b456c78de90f1a23b456c78de90f1a23b456c78 #replace it with your tokens
      - MULTIUSER=0  # default: 1
      - API_HASH=452b0359b988148995f22ff0f4229750  # get it from https://core.telegram.org/api/obtaining_api_id
```
# Tips & Tricks

- first follow [deployment guide](https://github.com/Rongronggg9/RSS-to-Telegram-Bot/blob/dev/docs/deployment-guide.md) to create a new bot,  get it's bot token, bot ID, telegraph api ID and telegraph api hash
- Uncomment MULTIUSER=0, if you want to be the only user that can interact with the bot and API_ID and API_HASH  if you don't want to use the sample APIs. API_ID_PUBLISHED_FLOOD_ERROR may occur with sample telegraph api's 

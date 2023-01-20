# RSS to telegram bot

A Telegram RSS bot that cares about your reading experience

- [GitHub repo](https://github.com/Rongronggg9/RSS-to-Telegram-Bot)
- [Docs](https://github.com/Rongronggg9/RSS-to-Telegram-Bot/tree/dev/docs)
- [env vars description](https://github.com/Rongronggg9/RSS-to-Telegram-Bot/blob/dev/docs/advanced-settings.md)

<br>

# docker-compose.yml

```yml
---
# Please read https://github.com/Rongronggg9/RSS-to-Telegram-Bot/blob/master/docs/deployment-guide.md for more details.
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

# ↓------ To disable sending via Telegraph, comment out this area ------↓ #
# Get Telegraph API access tokens: https://api.telegra.ph/createAccount?short_name=RSStT&author_name=Generated%20by%20RSStT&author_url=https%3A%2F%2Fgithub.com%2FRongronggg9%2FRSS-to-Telegram-Bot
# Refresh the page every time you get a new token.
# If you have a lot of subscriptions, make sure to get at least 5 tokens.
#                            ↓ Replace with your access tokens ↓
      - TELEGRAPH_TOKEN=
        1a23b456c78de90f1a23b456c78de90f1a23b456c78de90f1a23b456c78d
        1a23b456c78de90f1a23b456c78de90f1a23b456c78de90f1a23b456c78d
        1a23b456c78de90f1a23b456c78de90f1a23b456c78de90f1a23b456c78d
        1a23b456c78de90f1a23b456c78de90f1a23b456c78de90f1a23b456c78d
        1a23b456c78de90f1a23b456c78de90f1a23b456c78de90f1a23b456c78d
# ↑------ To disable sending via Telegraph, comment out this area ------↑ #

# Please read https://github.com/Rongronggg9/RSS-to-Telegram-Bot/blob/master/docs/advanced-settings.md for more details.
# ↓------ Advanced settings ------↓ #
      - MULTIUSER=0  # default: 1
      #- CRON_SECOND=30  # 0-59, default: 0
      #- DATABASE_URL=postgres://username:password@host:port/db_name  # default: sqlite://path/to/config/db.sqlite3
      - API_ID=1025907  # get it from https://core.telegram.org/api/obtaining_api_id
      - API_HASH=452b0359b988148995f22ff0f4229750  # get it from https://core.telegram.org/api/obtaining_api_id
      #- IMG_RELAY_SERVER=https://images.weserv.nl/?url=  # default: https://rsstt-img-relay.rongrong.workers.dev/
      #- IMAGES_WESERV_NL=https://t0.nl/  # default: https://images.weserv.nl/
      #- USER_AGENT=Mozilla/5.0 (Android 12; Mobile; rv:68.0) Gecko/68.0 Firefox/96.0  # default: RSStT/2.2 RSS Reader
      #- IPV6_PRIOR=1  # default: 0
      #- T_PROXY=socks5://172.17.0.1:1080  # Proxy used to connect to the Telegram API
      #- R_PROXY=socks5://172.17.0.1:1080  # Proxy used to fetch feeds
      #- PROXY_BYPASS_PRIVATE=1  # default: 0
      #- PROXY_BYPASS_DOMAINS=example.com;example.net
      #- HTTP_TIMEOUT=30  # default: 12
      #- HTTP_CONCURRENCY=0  # default: 1024
      #- HTTP_CONCURRENCY_PER_HOST=0  # default: 16
      #- TABLE_TO_IMAGE=1  # default: 0
      #- TRAFFIC_SAVING=1  # default: 0
      #- LAZY_MEDIA_VALIDATION=1  # default: 0
      #- MANAGER_PRIVILEGED=1  # default: 0
      #- NO_UVLOOP=1  # default: 0
      #- MULTIPROCESSING=1  # default: 0
      #- DEBUG=1  # debug logging, default: 0
# ↑------ Advanced settings ------↑ #
```
# Tips & Tricks

- first follow [deployment guide](https://github.com/Rongronggg9/RSS-to-Telegram-Bot/blob/dev/docs/deployment-guide.md) to create a new bot,  get it's bot token, bot ID, telegraph api ID and telegraph api hash
- Uncomment MULTIUSER=0, if you want to be the only user that can interact with the bot and API_ID and API_HASH  if you don't want to use the sample APIs. API_ID_PUBLISHED_FLOOD_ERROR may occur with sample telegraph api's 

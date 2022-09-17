# Change detection
Periodically monitors websites for changes and sends a notification when a change is detected.

- can use plain http request (faster option) or playwright (headless chrome) to render websites with javascript
- can filter a site with css selectors (to e.g. only get notifications when a price changes)
- many useful options like executing javascript on a site, before detecting changes (useful to eg. login)
- uses apprise for notifications, so hundreds of options (email, slack, telegram, sms, etc.)

<br>

- [Github repo](https://github.com/dgtlmoon/changedetection.io)
- [Non-self-hosted version](https://lemonade.changedetection.io/start)


## docker-compose.yml
```yml
---
services:
  playwright-chrome:
    hostname: playwright-chrome
    image: browserless/chrome
    restart: unless-stopped
    environment:
      - SCREEN_WIDTH=1920
      - SCREEN_HEIGHT=1024
      - SCREEN_DEPTH=16
      - ENABLE_DEBUGGER=false
      - PREBOOT_CHROME=true
      - CONNECTION_TIMEOUT=300000
      - MAX_CONCURRENT_SESSIONS=10
      - CHROME_REFRESH_TIME=600000
      - DEFAULT_BLOCK_ADS=true
      - DEFAULT_STEALTH=true
    volumes:
      - ./data:/datastore

  changedetection:
    image: ghcr.io/dgtlmoon/changedetection.io
    container_name: changedetection
    hostname: changedetection
    restart: unless-stopped
    environment:
      - PLAYWRIGHT_DRIVER_URL=ws://playwright-chrome:3000/?stealth=1&--disable-web-security=true
      - BASE_URL=https://changedetection.domain.com
    ports:
      - 5000:5000
```

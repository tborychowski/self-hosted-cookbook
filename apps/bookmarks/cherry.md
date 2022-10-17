# Cherry
- Not the worst looking
- Has dark theme
- Poor UX (e.g. no confirmations on user actions, loosing focus)
- Bookmarklet didn't work in safari
- New item is created in a popup (that does not have a deep-link to it, like in other services)

- [Github repo](https://github.com/haishanh/cherry)
- [Homepage](https://cherry.haishan.me)
- [Docs](https://cherry.haishan.me/docs/deploy)

![Screenshot](cherry.jpg)



## docker-compose.yml
```yml
---
services:
  cherry:
    image: haishanh/cherry
    container_name: cherry
    restart: unless-stopped
    environment:
      - JWT_SECRET=some-secret-string
      - ENABLE_PUBLIC_REGISTRATION=1
      - USE_INSECURE_COOKIE=1
    ports:
      - 3123:8000
    volumes:
      - ./data:/data
```

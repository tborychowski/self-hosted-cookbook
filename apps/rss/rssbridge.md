# RSS-Bridge

RSS-Bridge is a PHP project capable of generating RSS and Atom feeds for websites that don't have one.

<br>

- [GitHub repo](https://github.com/RSS-Bridge/rss-bridge)

# docker-compose.yml

```yml
---
services:
  rss-bridge:
    image: rssbridge/rss-bridge:latest
    volumes:
      - ./config:/config
    ports:
      - 3000:80
    restart: unless-stopped
```

# Tips & Tricks

:/config is mounted as config in the virent path.
Enable all bridges by default by creating a file named whitelist.txt in it and write an asterisk:

`echo '*' > whitelist.txt`

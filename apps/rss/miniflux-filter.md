# Miniflux-filter

Filter for miniflux - "mark as read" all unwanter articles.
I created that before miniflux added some basic filtering. Maybe still useful to someone.
The difference from the built-in filtering is that this marks articles as read, whereas the built-in filtering filters articles out BEFORE adding them to the DB.


<br>

- [Github repo](https://github.com/tborychowski/miniflux-filter)


## docker-compose.yml
```yml
---
version: '3'
services:
  miniflux-filter:
    image: tborychowski/miniflux-filter
    container_name: miniflux-filter
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
      - HOST=https://rss.example.com
      - API_KEY=<YOUR MINIFLUX API KEY>
      - CHECK_EVERY_S=300 # 300 seconds = 5 min
    ports:
      - "5011:3000"
    volumes:
      - ./filters.yml:/app/filters.yml
      - ./logs:/app/logs	# optional
```

## filters.yml
This is a simple list of matchers: it loops through the unread articles and for every one of them - it loops through this list. If the article URL matches a filter `url` and the article title contains the string from `title` field - this article will be marked as read.

```yml
filters:
  - url: 'feed.example.com'   # match feed url
    title: 'windows 10'       # match title
  - url: 'feed.example.com'
    title: 'sponsored'
  - url: 'feed.another.com'
    title: 'spam'
```

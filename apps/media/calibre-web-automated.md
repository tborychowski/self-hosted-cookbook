# Calibre-Web Automated

- all-in-one eBook manager (includes calibre server and an improved calibre-web fork)

<br>

- [Github repo](https://github.com/crocodilestick/calibre-web-automated)


## docker-compose.yml
```yml
services:
  calibre-web-automated:
    image: crocodilestick/calibre-web-automated:latest
    container_name: calibre-web-automated
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
      # Hardcover API Key required for Hardcover as a Metadata Provider,
      # get one here: https://docs.hardcover.app/api/getting-started/
      - HARDCOVER_TOKEN=your_hardcover_api_key_here
      - NETWORK_SHARE_MODE=true
      - DISABLE_LIBRARY_AUTOMOUNT=false
    ports:
      - 8083:8083
    volumes:
      - ./config:/config
      - ./library:/calibre-library
```

### Tips & Tricks

- Default Admin Login:
   - Username: admin
   - Password: admin123

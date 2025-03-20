# Mealie
- Pretty good configuration options & functionality
- Bulk entry for ingredients and preparation steps!
- Auto-import from URLs (provided they support Recipe Microdata)
- Excellent documentation
- Monotonous UI in a dreadful material style (poor UX)
- Not accessible nor keyboard friendly (poor UX)
- There are categories but you can't filter or group recipes (no tree-like navigation)

<br>

- [Github repo](https://github.com/hay-kot/mealie/)
- [Homepage](https://hay-kot.github.io/mealie/)

![Screenshot](mealie.png)


## docker-compose.yml
```yml
services:
  mealie:
    image: ghcr.io/mealie-recipes/mealie
    container_name: mealie
    restart: unless-stopped
    deploy:
      resources:
        limits:
          memory: 1000M # 
    environment:
      ALLOW_SIGNUP: "false"
      PUID: 1000
      PGID: 1000
      TZ: Europe/Dublin
      BASE_URL: https://mealie.yourdomain.com
    ports:
        - "9925:9000" # 
    volumes:
      - ./data:/app/data/
```

# Flame
- no-code configuration!
- clear UI and configurable (themes, custom CSS)
- nice features: app list, bookmark list
- add bookmark is not linkable (not possible to automate adding bookmarks)

<br>

- [Github repo](https://github.com/pawelmalak/flame)


![Screenshot](flame.png)

## docker-compose.yml
```yml
---
version: '3.6'

services:
  flame:
    image: pawelmalak/flame
    container_name: flame
    restart: unless-stopped
    ports:
      - 5005:5005
    environment:
      - PASSWORD=admin123
    volumes:
      - ./data:/app/data
      - /var/run/docker.sock:/var/run/docker.sock # optional but required for Docker integration
```

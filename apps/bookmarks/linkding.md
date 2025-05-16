# Linkding
- Looks very similar to pinboard
- Very simple and fast
- nice dark theme
- Firefox & Chrome extensions for adding bookmarks

- [Github repo](https://github.com/sissbruecker/linkding/)
- [Demo](https://demo.linkding.link/login/?next=/bookmarks)

![Screenshot](linkding.png)

## docker-compose.yml
```yml
services:
  linkding:
    image: sissbruecker/linkding:latest
    container_name: linkding
    restart: unless-stopped
    ports:
      - 3123:9090
    volumes:
      - ./data:/etc/linkding/data
```

Once it starts, create admin user:
```sh
docker-compose exec linkding python manage.py createsuperuser --username=joe --email=joe@example.com
```

## Tips & Tricks
Change password for a user
```sh
docker-compose exec linkding python manage.py changepassword <username>
```

# Koel

A complete music streaming solution that hosts and streams your own music collection.
- didn't manage to fetch any metadata for the tested music (not even covers embedded in the mp3!)
- allows making folders for playlists

<br>

- [Homepage](https://koel.dev/)
- [Github repo](https://github.com/koel/koel)
- [Docker repo](https://github.com/koel/docker)
- [Docs](https://docs.koel.dev/guide/what-is-koel)

## Env file
- save [.env.example](https://github.com/koel/koel/blob/master/.env.example) as `.env` next to the `docker-compose.yml` and when necessary.

## docker-compose.yml

```yml
services:
  database:
    image: mariadb:10.11
    container_name: koel-db
    restart: unless-stopped
    environment:
      - MYSQL_ROOT_PASSWORD=koelroot
      - MYSQL_DATABASE=koel
      - MYSQL_USER=koel
      - MYSQL_PASSWORD=koel
    volumes:
      - ./db:/var/lib/mysql

  koel:
    image: phanan/koel
    container_name: koel
    restart: unless-stopped
    depends_on:
      - database
    environment:
      - DB_CONNECTION=mysql
      - DB_HOST=database
      - DB_DATABASE=koel
      - DB_USERNAME=koel
      - DB_PASSWORD=koel
    ports:
      - 3123:80
    env_file: .env
    volumes:
      - ./sql:/docker-entrypoint-initdb.d
      - ./search_index:/var/www/html/storage/search-indexes
      - ./image_storage:/var/www/html/storage/app/public/images
      - ./music:/music
```

## Tips

### Init app:
```sh
docker compose exec --user www-data -it koel php artisan koel:init --no-assets
```

The first run creates a default admin account:
- Email: `admin@koel.dev`
- Password: `KoelIsCool`

Change this password immediately:
```sh
docker compose exec koel php artisan koel:admin:change-password
```

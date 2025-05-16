# Cachet

- focuses on posting information rather than pinging services
- looks like setting status is manual

<br>

- [Github repo](https://github.com/CachetHQ/Docker)


## docker-compose.yml
```yml
services:
    cachet:
        image: cachethq/docker:latest
        container_name: cachet
        restart: unless-stopped
        links:
            - postgres
        ports:
            - 3123:8000
        environment:
            - DB_DRIVER=pgsql
            - DB_HOST=postgres
            - DB_DATABASE=postgres
            - DB_USERNAME=postgres
            - DB_PASSWORD=postgres
            - APP_KEY=base64:tixFLbMoKffHKfUO1uEK9cGdpJxHYP7uCAp0lwwzEtM=

    postgres:
        image: postgres:9.5
        container_name: cachet-db
        restart: unless-stopped
        environment:
            - POSTGRES_USER=postgres
            - POSTGRES_PASSWORD=postgres
        volumes:
            - ./data:/var/lib/postgresql/data
```

## Tips & Tricks

### Generate APP_KEY
Once the container is running, execute the following command:
```sh
docker exec -i ID_OF_THE_CONTAINER php artisan key:generate
```
The full key should include `base64`, e.g.: `base64:YOUR_UNIQUE_KEY`.

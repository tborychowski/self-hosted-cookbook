# Screenly
Screenshotting service for NextCloud Bookmarks.

<br>

- [Homepage](http://screeenly.com/)
- [Github repo](https://github.com/stefanzweifel/screeenly)
- [DockerHub repo](https://hub.docker.com/r/hadogenes/screeenly)

## Setup
First run:
```sh
touch database.sqlite
chmod 777 database.sqlite
```

## .env
```ini
APP_NAME=screeenly
APP_ENV=local
APP_KEY=
APP_DEBUG=false
APP_URL=http://localhost
DB_CONNECTION=sqlite

# Disable Chrome's Sandbox feature
# More information: https://github.com/stefanzweifel/screeenly/issues/174#issuecomment-423438612
SCREEENLY_DISABLE_SANDBOX=true
SESSION_LIFETIME=1200
FILESYSTEM_DRIVER=public
```

## docker-compose.yml
```yml
---
version: '3'
services:
  screenly:
    image: hadogenes/screeenly
    container_name: screenly
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
    env_file:
      - ./.env
    ports:
      - "3110:80"
    volumes:
      - ./database.sqlite:/var/www/screeenly/database/database.sqlite
      - ./.env:/var/www/screeenly/.env
```

## Tips & Tricks
After starting the app - check the logs.
- If there are errors, log-in to container's shell:
    ```sh
    dc exec screenly sh
    ```
- If the errors are about the APP_KEY, inside the container, run:
    ```sh
    php artisan key:generate
    ```
- If there are errors about the DB records, run:
    ```sh
    php artisan migrate --force
    ```
Then register & log-in & generate API key for NextCloud.

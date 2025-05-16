# Monica

- Probably the only one in its kind :wink:
- Has a very nice feature: CardDAV, but it doesn't work with MacOS contacts :sad:

<br>

- [Homepage](https://www.monicahq.com/)
- [Github repo](https://github.com/monicahq/monica)


## Post-setup
Run this after starting the container for the first time:
```sh
docker-compose exec monicahq php artisan setup:production
```

## .env
```ini
APP_ENV=production
APP_DEBUG=false

# Must be 32 characters long exactly.
# Use `php artisan key:generate` or `pwgen -s 32 1` to generate a random key.
APP_KEY=ddDR0t8E666HH29tL2Fj281sJ2uh1WRQ
HASH_SALT=ddDRs8HH29t1UF2t8Evd
HASH_LENGTH=18
APP_URL=https://monica.example.com
APP_FORCE_URL=false
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=monica
DB_USERNAME=monica
DB_PASSWORD=monica01
DB_PREFIX=
DB_USE_UTF8MB4=true

# Mail credentials used to send emails from the application.
MAIL_MAILER=smtp
MAIL_HOST=
MAIL_PORT=25
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=
MAIL_FROM_NAME="CRM"
APP_EMAIL_NEW_USERS_NOTIFICATION=
APP_DISABLE_SIGNUP=true
APP_SIGNUP_DOUBLE_OPTIN=false
APP_TRUSTED_PROXIES=   # use a comma separated list of IP addresses.
APP_TRUSTED_CLOUDFLARE=false
LOG_CHANNEL=daily
SENTRY_SUPPORT=false
SENTRY_LARAVEL_DSN=
CHECK_VERSION=false
REDIS_HOST=redis
CACHE_DRIVER=redis     # database, file, memcached, redis, dynamodb
SESSION_DRIVER=redis   # file, cookie, database, apc, memcached, redis, array
SESSION_LIFETIME=120
QUEUE_CONNECTION=sync
DEFAULT_FILESYSTEM=public
DEFAULT_MAX_STORAGE_SIZE=512
DEFAULT_MAX_UPLOAD_SIZE=10240
MFA_ENABLED=true
DAV_ENABLED=true
ALLOW_STATISTICS_THROUGH_PUBLIC_API_ACCESS=false
POLICY_COMPLIANT=true
ENABLE_GEOLOCATION=false
LOCATION_IQ_API_KEY=
ENABLE_WEATHER=false
DARKSKY_API_KEY=
```

## docker-compose.yml
```yml
services:
  monicahq:
    image: monica
    container_name: monica
    restart: unless-stopped
    depends_on:
      - db
    env_file:
      - ./.env
    environment:
      - TZ=Europe/Dublin
      - APP_KEY=ddDR0t8E4s8HH19t333jt81sJNuh1WRQ
      - APP_ENV=production
      - DB_HOST=db
    ports:
      - "5020:80"
    volumes:
      - ./data:/var/www/html/storage
#      - ./html:/var/www/monica:ro
  db:
    image: mysql:5.7
    container_name: monica-db
    restart: unless-stopped
    environment:
      - MYSQL_RANDOM_ROOT_PASSWORD=true
      - MYSQL_DATABASE=monica
      - MYSQL_USER=monica
      - MYSQL_PASSWORD=monica01
    volumes:
      - ./mysql:/var/lib/mysql

  redis:
    image: redis:alpine
    container_name: monica-redis
    restart: unless-stopped
    volumes:
      - ./redis:/data
```

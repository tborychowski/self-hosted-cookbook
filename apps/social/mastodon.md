# Mastodon
Free, open-source social network server based on ActivityPub.

<br>

- [Homepage](https://joinmastodon.org)
- [Docs](https://docs.joinmastodon.org/)
- [LinuxServer docs](https://docs.linuxserver.io/images/docker-mastodon)


## Prerequisites
- To generate keys for `SECRET_KEY_BASE` & `OTP_SECRET` run the following command once for each:
```sh
docker run --rm -it --entrypoint /bin/bash lscr.io/linuxserver/mastodon generate-secret
```

- To generate keys for `VAPID_PRIVATE_KEY` & `VAPID_PUBLIC_KEY` run:

```sh
docker run --rm -it --entrypoint /bin/bash lscr.io/linuxserver/mastodon generate-vapid
```

- To use `tootctl` run:
```sh
# create account
docker compose exec mastodon /tootctl accounts create \
  admin \
  --email admin@example.com \
  --role Owner \
  --confirmed

# approve the account
docker compose exec mastodon /tootctl accounts approve admin
```



```yaml
services:
  redis:
    image: redis:alpine
    container_name: mastodon-redis
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
    volumes:
      - ./redis:/data

  db:
    image: postgres:latest
    environment:
      - POSTGRES_DB=mastodon
      - POSTGRES_USER=mastodon
      - POSTGRES_PASSWORD=mastodon

    volumes:
      - ./db:/var/lib/postgresql/data

  mastodon:
    image: lscr.io/linuxserver/mastodon:latest
    container_name: mastodon
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Dublin
      - LOCAL_DOMAIN=example.com
      - WEB_DOMAIN=mastodon.example.com #optional
	  - EMAIL_DOMAIN_ALLOWLIST=example.com|example.org

      - REDIS_HOST=redis
      - REDIS_PORT=6379

      - DB_HOST=db
      - DB_USER=mastodon
      - DB_NAME=mastodon
      - DB_PASS=mastodon
      - DB_PORT=5432

      - SECRET_KEY_BASE=
      - OTP_SECRET=

      - ES_ENABLED=false
      - S3_ENABLED=false

      - VAPID_PRIVATE_KEY=
      - VAPID_PUBLIC_KEY=

      - SMTP_SERVER=
      - SMTP_PORT=25
      - SMTP_LOGIN=
      - SMTP_PASSWORD=
      - SMTP_FROM_ADDRESS=mastodon@example.com

    volumes:
      - ./config:/config
    ports:
      - 3040:80
      - 3041:443
```

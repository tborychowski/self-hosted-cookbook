# Invidious

Invidious is an alternative front-end to YouTube.

<br>

- [Github repo](https://github.com/iv-org/invidious)

## Prerequisites
It requires some folders & files from the github repo, so easiest way is to clone the repo:
```sh
git clone https://github.com/iv-org/invidious.git
cd invidious/
```
and remove what's not needed:
```sh
rm -r .git/ .github/ assets/ kubernetes/ locales/ screenshots/ spec/ src/
rm .editorconfig .gitignore CHANGELOG.md  invidious.service LICENSE README.md shard.lock shard.yml TRANSLATION
rm docker-compose.yml  # removing this as we're gonna simplify it and let it use the prebuilt image from docker hub
```


## docker-compose.yml
```yml
version: '3'
services:
  postgres:
    image: postgres:10
    restart: unless-stopped
    volumes:
      - ./config/sql:/config/sql
      - ./docker/init-invidious-db.sh:/docker-entrypoint-initdb.d/init-invidious-db.sh
      - ./db:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: invidious
      POSTGRES_PASSWORD: kemal
      POSTGRES_USER: kemal
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "postgres"]
  invidious:
    image: omarroth/invidious
    restart: unless-stopped
    depends_on:
      - postgres
    ports:
      - "3000:3000"
    environment:
      INVIDIOUS_CONFIG: |
        channel_threads: 1
        check_tables: true
        feed_threads: 1
        db:
          user: kemal
          password: kemal
          host: postgres
          port: 5432
          dbname: invidious
        full_refresh: false
        https_only: false
        domain:
```


## Tips & Tricks

#### Remove the footer
After starting the container (before opening the site in the browser!) run:
```sh
docker-compose exec -u root invidious sh -c "echo \".footer{display:none;}\" >> /invidious/assets/css/default.css"
```

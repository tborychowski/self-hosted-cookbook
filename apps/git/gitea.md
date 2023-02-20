# Gitea

Gitea is your local git portal, similar to Github/Gitlab but lightweight and simple.
Can easily mirror repos from different sources (periodically re-syncing them), as a backup.

<br>

- [Homepage](https://gitea.io/en-us/)
- [Github repo](https://github.com/go-gitea/)
- [Docker guide](https://docs.gitea.io/en-us/install-with-docker/)


## docker-compose.yml
```yml
---
services:
  db:
    image: postgres:14
    container_name: gitea-db
    restart: unless-stopped
    environment:
      - POSTGRES_USER=gitea
      - POSTGRES_PASSWORD=gitea
      - POSTGRES_DB=gitea
    volumes:
      - ./postgres:/var/lib/postgresql/data

  server:
    image: gitea/gitea
    container_name: gitea
    restart: unless-stopped
    depends_on:
      - db
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - GITEA__database__DB_TYPE=postgres
      - GITEA__database__HOST=db:5432
      - GITEA__database__NAME=gitea
      - GITEA__database__USER=gitea
      - GITEA__database__PASSWD=gitea
      - GITEA__mailer__ENABLED=true
      - GITEA__mailer__FROM=git@domain.com
      - GITEA__mailer__MAILER_TYPE=smtp
      - GITEA__mailer__HOST=smtp.fastmail.com
      - GITEA__mailer__IS_TLS_ENABLED=true
      - GITEA__mailer__USER=tom@domain.com
      - GITEA__mailer__PASSWD=abcdefg_1234567890
    volumes:
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
      - ./gitea:/data
    ports:
      - "4000:3000"
      - "4022:22"
```

**Note:** Remember to put the correct URL in the first setup screen (including `http(s)`).

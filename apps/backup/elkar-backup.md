# Elkar Backup

Open source backup solution based on RSync/RSnapshot.
- Decent & clean UI
- Notification scripts
- Really easy to use


<br>

- [Github repo](https://github.com/elkarbackup/elkarbackup)
- [Docker Hub](https://hub.docker.com/r/elkarbackup/elkarbackup/)
- [Docs](https://docs.elkarbackup.org/docs/getting-started.html)
- [Homepage](https://www.elkarbackup.org/)


## docker-compose.yml
```yml
services:
  elkarbackup:
    image: elkarbackup/elkarbackup
    restart: unless-stopped
    environment:
      EB_CRON: "enabled"
      TZ: "Europe/Dublin"
      SYMFONY__DATABASE__PASSWORD: "123123123"
      SYMFONY__MAILER__TRANSPORT: "smtp"
      SYMFONY__MAILER__HOST: "mail.example.com"
      SYMFONY__MAILER__USER: "home@example.com"
      SYMFONY__MAILER__PASSWORD: "123123123123"
      SYMFONY__MAILER__FROM: "home@example.com"

    volumes:
      - ./uploads:/app/uploads
      - ./sshkeys:/app/.ssh
      - /mnt/backup:/paths/backups        # backup target
      - /mnt/docker:/paths/docker:ro    # backup source
      - ~:/paths/home:ro                # backup source
    ports:
      - 3070:80

  db:
    image: mysql:5.7.22
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: "1NXXQ5U8Uw6611Y70MSWvU0Vw"
    volumes:
      - ./db:/var/lib/mysql

  client:
    image: elkarbackup/client
    restart: unless-stopped
```

# Mattermost
Collaboration for Mission-Critical Work. Slack alternative.

<br>

- [Homepage](https://mattermost.com/)
- [Docs](https://docs.mattermost.com)
- [GH repo for docker setup](https://github.com/mattermost/docker)


```yaml
services:
  postgres:
    image: postgres:13-alpine
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    pids_limit: 100
    read_only: true
    tmpfs:
      - /tmp
      - /var/run/postgresql
    volumes:
      - ./database:/var/lib/postgresql/data
    environment:
      - TZ=Europe/Dublin
      - POSTGRES_USER=mattermost
      - POSTGRES_PASSWORD=mattermost
      - POSTGRES_DB=mattermost

  mattermost:
    depends_on:
      - postgres
    image: mattermost/mattermost-team-edition:9.11.6
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    pids_limit: 200
    read_only: false
    tmpfs:
      - /tmp
    volumes:
      - ./config:/mattermost/config:rw
      - ./data:/mattermost/data:rw
      - ./logs:/mattermost/logs:rw
      - ./plugins:/mattermost/plugins:rw
      - ./client-plugins:/mattermost/client/plugins:rw
      - ./bleve:/mattermost/bleve-indexes:rw
    environment:
      - TZ=Europe/Dublin
      - MM_SQLSETTINGS_DRIVERNAME=postgres
      - MM_SQLSETTINGS_DATASOURCE=postgres://mattermost:mattermost@postgres:5432/mattermost?sslmode=disable&connect_timeout=10
      - MM_SERVICESETTINGS_SITEURL=https://mm.domain.com

    ports:
      - 3123:8065     # Main App port. Open this in browser
      - 8443:8443/udp
      - 8443:8443/tcp
```


## Tips & Tricks
- If logs show permission errors, it may be required that the local folders need write permissions.
the easiest thing to do is to run the following, in the same directory as the `docker-compose.yml` file:
```sh
chmod -R 777 ./config ./data ./logs ./plugins ./client-plugins ./bleve
```

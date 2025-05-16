# n8n

n8n is a free and open node-based Workflow Automation Tool. It can be self-hosted, easily extended, and so also used with internal tools.

---

- [Homepage](https://n8n.io/)
- [Github repo](https://github.com/n8n-io/n8n)
- [Docs](https://docs.n8n.io/)


## Setup
Create 1 folder and 3 files in the same directory:
- storage/
- .env
- init-data.sh
- docker-compose.yml

```sh
touch .env init-data.sh docker-compose.yml
mkdir storage

chmod +x init-data.sh  # make the init-data.sh executable
chmod 777 storage      # make sure the storage folder is writable
```



## .env
```conf
POSTGRES_USER=n8n
POSTGRES_PASSWORD=n8n
POSTGRES_DB=n8n

POSTGRES_NON_ROOT_USER=n7n
POSTGRES_NON_ROOT_PASSWORD=n7n
```


## init-data.sh
```sh
#!/bin/bash
set -e;

if [ -n "${POSTGRES_NON_ROOT_USER:-}" ] && [ -n "${POSTGRES_NON_ROOT_PASSWORD:-}" ]; then
	psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" <<-EOSQL
		CREATE USER ${POSTGRES_NON_ROOT_USER} WITH PASSWORD '${POSTGRES_NON_ROOT_PASSWORD}';
		GRANT ALL PRIVILEGES ON DATABASE ${POSTGRES_DB} TO ${POSTGRES_NON_ROOT_USER};
		GRANT CREATE ON SCHEMA public TO ${POSTGRES_NON_ROOT_USER};
	EOSQL
else
	echo "SETUP INFO: No Environment variables given!"
fi
```


## docker-compose.yml
```yml
services:
  postgres:
    image: postgres:16
    restart: unless-stopped
    environment:
      - POSTGRES_USER
      - POSTGRES_PASSWORD
      - POSTGRES_DB
      - POSTGRES_NON_ROOT_USER
      - POSTGRES_NON_ROOT_PASSWORD
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -h localhost -U ${POSTGRES_USER} -d ${POSTGRES_DB}']
      interval: 5s
      timeout: 5s
      retries: 10
    volumes:
      - ./db:/var/lib/postgresql/data
      - ./init-data.sh:/docker-entrypoint-initdb.d/init-data.sh

  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: unless-stopped
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=${POSTGRES_DB}
      - DB_POSTGRESDB_USER=${POSTGRES_NON_ROOT_USER}
      - DB_POSTGRESDB_PASSWORD=${POSTGRES_NON_ROOT_PASSWORD}
      - N8N_EDITOR_BASE_URL=http://localhost:5678/
      #- N8N_SECURE_COOKIE=false
    links:
      - postgres
    depends_on:
      postgres:
        condition: service_healthy
    ports:
      - 5678:5678
    volumes:
      - ./storage:/home/node/.n8n
```



## Tips & Tricks
When running through a reverse proxy, there's a couple of env vars needed (it's a bit weird):
- Normally you'd provide a url, that the app should expect and should set when generating links.
- However, here `N8N_EDITOR_BASE_URL` is not sufficient, and doesn't look like it's working for this, so the host, port and protocol are still required.
- However 2, if you set the port to 443 (as you will be accessing the app via your reverse proxy & https) - the app itself will run on this port (instead of the default: 5678), so you also need to change the container's internal port to 443.


```yml
     environment:
       - N8N_HOST=n8n.mydomain.com
       - N8N_PORT=443
       - N8N_PROTOCOL=https
       - N8N_EDITOR_BASE_URL=https://n8n.mydomain.com
     ports:
       - 5678:443
```

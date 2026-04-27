# Kaneo

- Simple self-hosted project management tool with kanban boards and gantt charts.
- The UI is pretty nice and intuitive, and it has all the basic features you'd expect from a project management tool (tasks, subtasks, comments, attachments, etc.).
- Keyboard support is a bit "opinionated" so may require some getting used to, but overall it's pretty comprehensive.
- It seems to also have a 2-way github issues sync (haven't tried it yet).
- The docs are a bit vague in some areas, which made the setup more challenging than necessary.


<br>

- [Homepage](https://kaneo.app/)
- [Github repo](https://github.com/usekaneo/kaneo)
- [Docs](https://kaneo.app/docs)



## docker-compose.yml
```yaml
services:
  postgres:
    image: postgres:16-alpine
    restart: unless-stopped
    environment:
      POSTGRES_DB: kaneo
      POSTGRES_USER: kaneo_user
      POSTGRES_PASSWORD: kaneo_password
    volumes:
      - ./data:/var/lib/postgresql/data

  backend:
    image: ghcr.io/usekaneo/api:latest
    container_name: kaneoapi
    restart: unless-stopped
    depends_on:
      - postgres
    environment:
      AUTH_SECRET: "UPWSUr1YriZXz8CRV5Ecqs3iRudCQ2syOtyzz6Buf3k="
      DATABASE_URL: "postgresql://kaneo_user:kaneo_password@postgres:5432/kaneo"
      KANEO_API_URL: "http://<SERVER_IP>:1337"
      KANEO_CLIENT_URL: "http://<SERVER_IP>:5173"
      CORS_ORIGINS: "http://<SERVER_IP>:5173,http://<SERVER_IP>:1337"
    ports:
      - 1337:1337

  frontend:
    image: ghcr.io/usekaneo/web:latest
    restart: unless-stopped
    depends_on:
      - backend
    environment:
      KANEO_API_URL: "http://<SERVER_IP>:1337"
      KANEO_CLIENT_URL: "http://<SERVER_IP>:5173"
      CORS_ORIGINS: "http://<SERVER_IP>:5173,http://<SERVER_IP>:1337"
    ports:
      - 5173:5173
```


## Tips & Tricks
- Generate the `AUTH_SECRET` using `openssl rand -base64 32`.
- Replace `<SERVER_IP>` with the IP/hostname of your server.
- ensure that the ports match (especially in both `CORS_ORIGINS` variables as without this - the client and server won't be able to communicate).
- The app will be available at `http://<SERVER_IP>:5173` (front-end).

# Planka

- [Homepage](https://planka.app/)
- [Github repo](https://github.com/plankanban/planka)


## docker-compose.yml
```yml
services:
  planka:
    image: meltyshev/planka:latest
    command: >
      bash -c
        "for i in `seq 1 30`; do
          ./start.sh &&
          s=$$? && break || s=$$?;
          echo \"Tried $$i times. Waiting 5 seconds...\";
          sleep 5;
        done; (exit $$s)"
    restart: unless-stopped
    volumes:
      - ./avatars:/app/public/user-avatars
      - ./background-images:/app/public/project-background-images
      - ./attachments:/app/public/attachments
    ports:
      - 3123:1337
    environment:
      - BASE_URL=http://localhost:3123
      - DATABASE_URL=postgresql://postgres@postgres/planka
      - SECRET_KEY=VCFh2BiD6N202jR2jYcp22FqfJfaUg2omTB2MlAZp7o=
    depends_on:
      - postgres

  postgres:
    image: postgres:12-alpine
    restart: unless-stopped
    volumes:
      - ./db:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=planka
      - POSTGRES_HOST_AUTH_METHOD=trust
```


## Tips & Tricks
Login with:
- email: `demo@demo.demo`
- pass: `demo`

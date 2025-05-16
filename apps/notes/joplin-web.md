## Jolin-web

Joplin-vieweb purpose is to provide an online view of Joplin notes.
It's running on a "Django server", running beside Joplin terminal app.

## Requirements

A Joplin server to that stores your notes

<br>

- [Github repo](https://github.com/joplin-vieweb)
- [Docs](https://github.com/joplin-vieweb/joplin-vieweb/#installation--configuration-instructions)

## docker-compose.yml

```yml

x-common-variables: &common-variables
   ORIGINS: "'http://localhost', 'http://192.168.1.24' , 'https://my-ddns-domain.com'"

services:
  django-joplin-vieweb:
    image: gri38/django-joplin-vieweb:latest
    depends_on:
      - joplin-terminal-xapi
    environment:
       <<: *common-variables
    restart: unless-stopped
    ports:
      - xxxx:8000
    volumes:
      - joplin:/root/.config/joplin:ro
      - joplin-vieweb:/root/.config/joplin-vieweb
    networks:
      - joplin-net

  joplin-terminal-xapi:
    image: gri38/joplin-terminal-xapi:latest
    restart: unless-stopped
    volumes:
      - joplin:/root/.config/joplin
    networks:
      - joplin-net

volumes:
  joplin:
  joplin-vieweb:

networks:
  joplin-net: {}
```

## Tips & Tricks

###
You can access your notebooks: https://your_domain/joplin (⚠ don't forget the /joplin ⚠)

- first you set up url/admin and then login to url/joplin
mind the /admin and /joplin

- for webdav sync you can select the nextcloud option

- To decryt the notes run

```sh
docker exec -ti joplin-vieweb_joplin-terminal-xapi_1 joplin e2ee decrypt
```

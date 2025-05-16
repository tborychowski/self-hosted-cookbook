# Memos
An open-source, self-hosted memo hub with knowledge management and collaboration.

- Really easy to setup and use
- Has a dedicated mobile client!
- Quite good UX

<br>

- [Homepage](https://usememos.com)
- [Github repo](https://github.com/usememos/memos)
- [Mobile client](https://memos.moe)


## docker-compose.yml
```yaml
services:
  memos:
    image: neosmemo/memos:latest
    container_name: memos
    volumes:
      - ./memos:/var/opt/memos
    ports:
      - 5230:5230
```

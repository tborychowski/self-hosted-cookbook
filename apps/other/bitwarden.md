# Bitwarden
Bitwarden is a password manager & vault.<br>

bitwarden_rs is an unofficial Bitwarden compatible server.

<br>

- [Official Bitwarden Site](https://bitwarden.com/)
- [bitwarden_rs Github repo](https://github.com/dani-garcia/bitwarden_rs)
- [bitwarden_rs Docs](https://github.com/dani-garcia/bitwarden_rs/wiki)


## docker-compose.yml
```yml
---
version: '3'
services:
  bitwarden:
    image: bitwardenrs/server:latest
    container_name: bitwarden
    restart: unless-stopped
    ports:
      - "3000:80"
    volumes:
      - ./data:/data/
    environment:
      - SIGNUPS_ALLOWED=false
	  - ADMIN_TOKEN=123123123123123123123123123123123123123123
```

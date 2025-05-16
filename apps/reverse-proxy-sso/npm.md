# nginx-proxy-manager

- it's basically a very nice UI for nginx reverse-proxy
- a very nice UI
- making it extremely easy to use nginx

<br>

- [Github repo](https://github.com/jc21/nginx-proxy-manager)
- [Homepage](https://nginxproxymanager.com/guide/#quick-setup)



## docker-compose.yml
```yml
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

Login with:
- Email: admin@example.com
- Password: changeme

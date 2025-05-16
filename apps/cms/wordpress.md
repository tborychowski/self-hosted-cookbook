# Wordpress
The ultimate CMS/blogging platform.
- hellishly slow! (loading from a server in LAN takes ~3-7 seconds)
- extremely complex
- tons of plugins & themes & configurability

<br>

- [Homepage](https://wordpress.org/)
- [Docker Hub](https://hub.docker.com/_/wordpress)
- [Github repo](https://github.com/WordPress/WordPress)


## docker-compose.yml
```yml
services:
  wordpress:
    image: wordpress
    restart: unless-stopped
    ports:
      - 3123:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: blog
    volumes:
      - ./data:/var/www/html

  db:
    image: mysql:5.7
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: blog
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - ./db:/var/lib/mysql
```

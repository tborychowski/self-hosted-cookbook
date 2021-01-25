# Mealie
- Pretty good configuration options & functionality
- Bulk entry for ingredients and preparation steps!
- Auto-import from URLs (provided they support Recipe Microdata)
- Excellent documentation
- Monotonous UI in a dreadful material style (poor UX)
- Not accessible nor keyboard friendly (poor UX)
- There are categories but you can't filter or group recipes (no tree-like navigation)

<br>

- [Github repo](https://github.com/hay-kot/mealie/)
- [Homepage](https://hay-kot.github.io/mealie/)

![Screenshot](mealie.png)


## docker-compose.yml
```yml
version: '3.3'
services:
  dashmachine:
    image: rmountjoy/dashmachine:latest
    container_name: dashmachine
    restart: unless-stopped
    ports:
      - 4010:5000
    volumes:
      - ./data:/dashmachine/dashmachine/user_data
```

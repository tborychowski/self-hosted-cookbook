# OpenSpeedTest
Pure HTML5 Network Performance Estimation Tool.<br>
Measure the speed between your server and your computer.

<br>

- [Homepage](http://openspeedtest.com)
- [Github repo](https://github.com/openspeedtest/Speed-Test)

--name openspeedtest -d -p 3000:3000 -p 3001:3001

## docker-compose.yml
```yml
---
services:
  openspeedtest:
    image: openspeedtest/latest
    container_name: openspeedtest
    restart: unless-stopped
    ports:
      - "3000:3000"
```

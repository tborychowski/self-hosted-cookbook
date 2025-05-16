# RocketChat

The Ultimate Communication Hub. A solid option for self-hosted chat and collaboration.

- [Homepage](https://www.rocket.chat)
- [Documentation](https://docs.rocket.chat/docs/deploy-with-docker-docker-compose)
- [Github repo](https://github.com/rocketchat/)



<br>


## docker-compose.yml
```yml
services:
  mongodb:
    # https://hub.docker.com/r/bitnami/mongodb
    image: docker.io/bitnami/mongodb:8.0.5
    restart: unless-stopped
    volumes:
      - ./data:/bitnami/mongodb
    environment:
      MONGODB_REPLICA_SET_MODE: primary
      MONGODB_REPLICA_SET_NAME: rs0
      MONGODB_PORT_NUMBER: 27017
      MONGODB_INITIAL_PRIMARY_HOST: mongodb
      MONGODB_INITIAL_PRIMARY_PORT_NUMBER: 27017
      MONGODB_ADVERTISED_HOSTNAME: mongodb
      MONGODB_ENABLE_JOURNAL: true
      ALLOW_EMPTY_PASSWORD: yes


  rocketchat:
    # https://github.com/RocketChat/Rocket.Chat/releases
    image: registry.rocket.chat/rocketchat/rocket.chat:7.4.0
    restart: unless-stopped
    environment:
      MONGO_URL: "mongodb://mongodb:27017/rocketchat?replicaSet=rs0"
      MONGO_OPLOG_URL: "mongodb://mongodb:27017/local?replicaSet=rs0"
      ROOT_URL: http://localhost:3123
      PORT: 3000
      DEPLOY_METHOD: docker
    depends_on:
      - mongodb
    expose:
      - 3000
    ports:
      - 3123:3000
```

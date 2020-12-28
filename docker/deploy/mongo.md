# mongo docker service 部署

## 目录说明

```text

-# mongo
---- data
---- docker-compose.yml
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
```

_docker-compose.yml_

```yml

version: '3.4'
services:
  mongo:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes: 
      - $PWD/data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: learn
      MONGO_INITDB_ROOT_PASSWORD: learn-m
    networks:
      - learn-network
  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - "18181:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: learn
      ME_CONFIG_MONGODB_ADMINPASSWORD: learn-m
    networks:
      - learn-network
networks:
  learn-network:
    external: 
      name: learn-network
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml mongo
```


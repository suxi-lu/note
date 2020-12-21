# redis docker service 部署

## 目录说明

```text

-# redis
---- data
---- docker-compose.yml
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
```

_docker-compose.yml_

```text

version: '3.4'
services:
  redis:
    image: redis:latest
    ports:
      - "16379:6379"
    volumes: 
      - $PWD/data:/data
    networks:
      - learn-network
networks:
  learn-network:
    external: 
      name: learn-network
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml redis
```


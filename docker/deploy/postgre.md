# postgre docker service 部署

## 目录说明

```text

-# postgre
---- data
---- docker-compose.yml
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.26.1 --subnet 10.0.26.0/24 postgre-network
```

_docker-compose.yml_

```yml
version: '3.1'
services:
  db:
    image: postgres:9.6.20-alpine
    restart: always
    ports:
      - 15432:5432
    environment:
      POSTGRES_PASSWORD: password
    volumes:
      - ./data:/var/lib/postgresql/data
    networks:
      - postgre-network
  adminer:
    image: adminer:4.7.8-standalone
    restart: always
    ports:
      - 18080:8080
    networks:
      - postgre-network
networks:
  postgre-network:
    external: 
      name: postgre-network
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml postgre
```

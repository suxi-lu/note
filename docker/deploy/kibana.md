# kibana docker service 部署

## 目录说明

```text

-# kibana
---- docker-compose.yml
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.7.1 --subnet 10.0.7.0/24 elk-network
```

_docker-compose.yml_

```yml

version: '3.4'
services:
  kibana:
    image: kibana:latest
    ports:
      - "15601:5601"
    environment:
      ELASTICSEARCH_URL: http://:
    networks:
      - elk-network
networks:
  elk-network:
    external: 
      name: elk-network
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml kibana
```


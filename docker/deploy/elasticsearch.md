# elasticsearch docker service 部署

## 目录说明

```text

-# elasticsearch
---- conf
---- data
---- log
---- docker-compose.yml
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.7.1 --subnet 10.0.7.0/24 elk-network
```

_docker-compose.yml_

```text

version: '3.4'
services:
  elasticsearch:
    image: elasticsearch:latest
    ports:
      - "19200:9200"
      - "19300:9300"
    volumes: 
      #- $PWD/conf:/usr/share/elasticsearch/config
      - $PWD/log:/usr/share/elasticsearch/logs
      - $PWD/data:/usr/share/elasticsearch/data
    environment:
      cluster.name: es-master
      http.host: 0.0.0.0
      node.name: master
      node.master: 1 
      node.data: 1
      bootstrap.memory_lock: 1

      http.cors.enabled: 1
      http.cors.allow-origin: "*"
      transport.host: 0.0.0.0
      transport.tcp.port: 19300
      network.publish_host: 
    networks:
      - elk-network
networks:
  elk-network:
    external: 
      name: elk-network
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml elasticsearch
```


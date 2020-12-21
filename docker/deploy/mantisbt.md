# mantisbt docker service 部署\(建议使用5.7mysql版本\)

## 目录说明

```text

-# mysql
---- config
---- custom
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
  php-mantisbt:
    image: xlrl/mantisbt:latest
    ports:
      - "19001:80"
    volumes: 
      #- $PWD/mantisbt-2.17.0/config:/var/www/html/config
      - $PWD/config:/var/lib/www/html/config
      - $PWD/custom:/var/lib/www/html/custom
    networks:
      - learn-network
networks:
  learn-network:
    external: 
      name: learn-network
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml mantisbt
```


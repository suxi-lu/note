# nexus docker service 部署

## 准备工作

```text

[root nexus]$ mkdir nexus-data && chown -R 200 nexus-data
```

## 目录说明

```text

-# nexus
---- nexus-data (建议目录不要更换其他名称)
---- docker-compose.yml
---- test.sh
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.26.1 --subnet 10.0.26.0/24 nexus-network
```

_docker-compose.yml_

```yml

version: '3.4'
services:
  nexus:
    image: sonatype/nexus3:latest
    ports:
      - "18081:8081"
    volumes: 
      - $PWD/nexus-data:/nexus-data
    networks:
      - nexus-network
networks:
  nexus-network:
    external: 
      name: nexus-network
```

_test.sh_

```text

$ curl -u admin:admin123 http://127.0.0.1:18081/service/metrics/ping
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml nexus
```

启动成功后执行`$ ./test.sh`返回`pong`说明启动成功  
访问[`http://ip:port`](http://ip:port)仓库管理页面


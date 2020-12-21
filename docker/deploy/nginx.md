# nginx docker service 部署

## 目录说明

```text

-# nginx
---- conf
------ blog.conf
---- log
---- docker-compose.yml
```

_blog.conf_

```text

server {
    listen       80;
    server_name  blog.suxilu.cn;

    location / {
        proxy_pass http://blog.suxilu.cn:10088/;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
```

_docker-compose.yml_

```text

version: '3.4'
services:
  nginx:
    image: nginx:latest
    restart: always
    ports:
      - "80:80"
    volumes: 
      - ./conf:/etc/nginx/conf.d
      - ./log:/var/log/nginx
    environment:
      - NGINX_PORT=80
#    networks:
#      - learn-network
#networks:
#  learn-network:
#    external: 
#      name: learn-network
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml nginx
```


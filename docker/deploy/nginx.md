# nginx docker service 布署  

#### 目录说明  

<pre><code>
-# nginx
---- conf
------ blog.conf
---- log
---- docker-compose.yml
</code></pre>

*blog.conf*
 
<pre><code>
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
</code></pre>

*docker network create*  
<pre><code>
docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
</code></pre>

*docker-compose.yml*

<pre><code>
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
</code></pre>

*启动命令*
<pre><code>
$ docker stack deploy -c docker-compose.yml nginx
</code></pre>
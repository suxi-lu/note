# redis docker service 布署

#### 目录说明  

<pre><code>
-# redis
---- data
---- docker-compose.yml
</code></pre>

*docker network create*  
<pre><code>
docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
</code></pre>

*docker-compose.yml*

<pre><code>
version: '3.4'
services:
  redis:
    image: redis:latest
    #restart: always
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
</code></pre>

*启动命令*
<pre><code>
$ docker stack deploy -c docker-compose.yml redis
</code></pre>
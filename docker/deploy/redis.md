# redis docker service 布署

#### 目录说明  

<pre><code>
-# redis
---- data
-- docker-compose.yml
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
docker stack deploy -c docker-compose.yml redis
</code></pre>
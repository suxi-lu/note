# rabbitmq docker service 布署  
 
#### 目录说明  

<pre><code>
-# rabbitmq
---- data
-- docker-compose.yml
</code></pre>

*docker network create*  
<pre><code>
docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
</code></pre>

*docker-compose.yml*

<pre><code>
version: '3.4'
services:
  rabbitmq:
    image: rabbitmq:management
    #restart: always
    ports:
      - "5671:5671"
      - "5672:5672"
      - "4369:4369"
      - "25672:25672"
      - "15671:15671"
      - "15672:15672"
    volumes: 
      #- $PWD/conf/rabbitmq-my.config:/var/lib/rabbitmq/rabbitmq-my.config
      - $PWD/data:/var/lib/rabbitmq
    environment:
      #RABBITMQ_ERLANG_COOKIE: 
      RABBITMQ_DEFAULT_USER: guest
      RABBITMQ_DEFAULT_PASS: guest
    networks:
      - learn-network
networks:
  learn-network:
    external: 
      name: learn-network
</code></pre>

*启动命令*
<pre><code>
docker stack deploy -c docker-compose.yml rabbitmq
</code></pre>
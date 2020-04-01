# mongo docker service 部署  

#### 目录说明  

<pre><code>
-# mongo
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
  mongo:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes: 
      - $PWD/data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: learn
      MONGO_INITDB_ROOT_PASSWORD: learn-m
    networks:
      - learn-network
  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - "18181:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: learn
      ME_CONFIG_MONGODB_ADMINPASSWORD: learn-m
    networks:
      - learn-network
networks:
  learn-network:
    external: 
      name: learn-network
</code></pre>

*启动命令*
<pre><code>
$ docker stack deploy -c docker-compose.yml mongo
</code></pre>
# kibana docker service 部署  

#### 目录说明  

<pre><code>
-# kibana
---- docker-compose.yml
</code></pre>

*docker network create*  
<pre><code>
docker network create -d overlay --gateway 10.0.7.1 --subnet 10.0.7.0/24 elk-network
</code></pre>

*docker-compose.yml*

<pre><code>
version: '3.4'
services:
  kibana:
    image: kibana:latest
    ports:
      - "15601:5601"
    environment:
      ELASTICSEARCH_URL: http://<host>:<port>
    networks:
      - elk-network
networks:
  elk-network:
    external: 
      name: elk-network
</code></pre>

*启动命令*
<pre><code>
$ docker stack deploy -c docker-compose.yml kibana
</code></pre>
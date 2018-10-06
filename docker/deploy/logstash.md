# logstash docker service 布署(待完善)  

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
  logstash:
    image: logstash:latest
    #restart: always
    ports:
      - "19600:9600"
      - "14560:14560"
    volumes: 
      #- $PWD/logstash.yml:/usr/share/logstash/config/logstash.yml:ro
      - $PWD/pipeline:/usr/share/logstash/pipeline
    environment:
      http.host: 0.0.0.0
      path.config: /usr/share/logstash/pipeline/*
      xpack.monitoring.elasticsearch.url: http://<host>:<port>
    networks:
      - elk-network
networks:
  elk-network:
    external: 
      name: elk-network
</code></pre>

*启动命令*
<pre><code>
$ docker stack deploy -c docker-compose.yml logstash
</code></pre>
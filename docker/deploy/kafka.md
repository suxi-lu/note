# kafka docker service 部署  

#### 目录说明  

<pre><code>
-# kafka
---- docker-compose.yml
</code></pre>

*docker-compose.yml*

<pre><code>
version: '3.4'
services:
  zookeeper:
    #image: wurstmeister/zookeeper
    image: zookeeper:3.6.1
    ports:
      - "2181:2181"
  kafka:
    image: wurstmeister/kafka:2.12-2.5.0
    ports:
      - "9092:9092"
    environment:
      KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:9092
      KAFKA_ADVERTISED_HOST_NAME: <hostname>
      KAFKA_CREATE_TOPICS: "test:1:1"
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
  kafdrop:
    image: obsidiandynamics/kafdrop:3.27.0
    ports:
      - "9000:9000"
    environment:
      KAFKA_BROKERCONNECT: kafka:9092
      JVM_OPTS: "-Xms32M -Xmx64M"
      SERVER_SERVLET_CONTEXTPATH: "/"
</code></pre>

*启动命令*
<pre><code>
$ docker-compose up -d
</code></pre>
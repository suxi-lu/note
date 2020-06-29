# flink docker service 部署  

#### 目录说明  

<pre><code>
-# flink
---- docker-compose.yml
</code></pre>

*docker-compose.yml*

<pre><code>
version: '3.4'
services:
  jobmanager:
    image: flink:1.10.1-scala_2.12
    expose:
      - "6123"
    ports:
      - "18081:8081"
    command: jobmanager
    environment:
      - JOB_MANAGER_RPC_ADDRESS=jobmanager
  taskmanager:
    image: flink:1.10.1-scala_2.12
    expose:
      - "6121"
      - "6122"
    depends_on:
      - jobmanager
    command: taskmanager
    links:
      - "jobmanager:jobmanager"
    environment:
      - JOB_MANAGER_RPC_ADDRESS=jobmanager
</code></pre>

*启动命令*
<pre><code>
$ docker-compose up -d
</code></pre>
# nexus docker service 布署  

#### 准备工作

<pre><code>
[root nexus]$ mkdir nexus-data && chown -R 200 nexus-data
</code></pre>

#### 目录说明  

<pre><code>
-# nexus
---- nexus-data (建议目录不要更换其他名称)
---- docker-compose.yml
---- test.sh
</code></pre>

*docker network create*
<pre><code>
docker network create -d overlay --gateway 10.0.26.1 --subnet 10.0.26.0/24 nexus-network
</code></pre>

*docker-compose.yml*

<pre><code>
version: '3.4'
services:
  nexus:
    image: sonatype/nexus3:latest
    ports:
      - "18081:8081"
    volumes: 
      - $PWD/nexus-data:/nexus-data
    networks:
      - nexus-network
networks:
  nexus-network:
    external: 
      name: nexus-network
</code></pre>

*test.sh*

<pre><code>
$ curl -u admin:admin123 http://127.0.0.1:18081/service/metrics/ping
</code></pre>

安装成功后执行<code>$ ./test.sh</code>返回<code>pong</code>说明安装成功  
访问<code>http://ip:port</code>仓库管理页面
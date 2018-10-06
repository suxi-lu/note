# mysql docker service 布署  

#### 目录说明  

<pre><code>
-# mysql
---- conf
------ my.cnf
---- data
---- docker-compose.yml
</code></pre>

*my.cnf*
 
<pre><code>
[mysql]
default-character-set=utf8

[mysqld]
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock

symbolic-links=0

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

lower_case_table_names=1
init_connect='SET collation_connection = utf8_general_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_general_ci
skip-character-set-client-handshake
default-authentication-plugin=mysql_native_password
#innodb_buffer_pool_size=30M #->256M InnoDB引擎缓冲区占了大头，首要就是拿它开刀
#query_cache_size=16M        #->16M 查询缓存
#tmp_table_size=10M          #->64M 临时表大小
#key_buffer_size=24m         #->32M

performance_schema_max_table_instances=100
table_definition_cache=100
table_open_cache=56

[client]
default-character-set=utf8

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
</code></pre>

*docker network create*  
<pre><code>
docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
</code></pre>

*docker-compose.yml*

<pre><code>
version: '3.4'
services:
  mysql:
    image: mysql:latest
    #restart: always
    ports:
      - "3306:3306"
    volumes: 
      - $PWD/conf/my.cnf:/etc/mysql/mysql.conf.d
      #- ./log:/var/log
      - $PWD/data:/var/lib/mysql
    environment:
      TZ: 'Asia/Shanghai'
      MYSQL_ROOT_PASSWORD: root
    networks:
      - learn-network
networks:
  learn-network:
    external: 
      name: learn-network
</code></pre>

*启动命令*
<pre><code>
$ docker stack deploy -c docker-compose.yml mysql
</code></pre>
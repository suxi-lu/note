# mysql docker service 部署

## 目录说明

```text

-# mysql
---- conf
------ my.cnf
---- data
---- docker-compose.yml
```

_my.cnf_

```text

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
```

_docker network create_

```text

docker network create -d overlay --gateway 10.0.6.1 --subnet 10.0.6.0/24 learn-network
```

_docker-compose.yml_

```yml

version: '3.4'
services:
  mysql:
    image: mysql:latest
    ports:
      - "3306:3306"
    volumes: 
      #- $PWD/conf/my.cnf:/etc/mysql/my.cnf    # 5.7配置
      - $PWD/conf/my.cnf:/etc/mysql/mysql.conf.d    # 8.*配置
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
```

_启动命令_

```text

$ docker stack deploy -c docker-compose.yml mysql
```


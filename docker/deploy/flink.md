# flink docker service 部署

## 目录说明

```text

-# flink
---- docker-compose.yml
```

_docker-compose.yml_

```yml

version: '3.4'
services:
  jobmanager:
    image: flink:1.10.1-scala_2.12
    expose:
      - "6123"
    ports:
      - "18081:8081"
    command: jobmanager
    volumes:
      - /host/path/to/job/artifacts:/opt/flink/usrlib
    environment:
      - |
        FLINK_PROPERTIES=
        jobmanager.rpc.address: jobmanager
        parallelism.default: 3

  taskmanager:
    image: flink:1.10.1-scala_2.12
    expose:
      - "6121"
      - "6122"
    depends_on:
      - jobmanager
    command: taskmanager
    volumes:
      - /host/path/to/job/artifacts:/opt/flink/usrlib
    links:
      - "jobmanager:jobmanager"
    environment:
      - |
        FLINK_PROPERTIES=
        jobmanager.rpc.address: jobmanager
        taskmanager.numberOfTaskSlots: 3
        parallelism.default: 3
```

_启动命令_

```text

$ docker-compose up -d
```


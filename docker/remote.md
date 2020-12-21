# docker 配置远程访问

1、主要是在\[Service\]这个部分，加上下面参数  
vi /usr/lib/systemd/system/docker.service

```text

[Service]
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

2、docker重新读取配置文件，重新启动docker服务

```text

$ systemctl daemon-reload  
$ systemctl restart docker
```

3、查看docker进程，发现docker守护进程在已经监听2375端口

```text

$ ps -ef|grep docker
```


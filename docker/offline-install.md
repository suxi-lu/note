# Linux下离线安装Docker

一、 基础环境

1. 操作系统：CentOS 7.3  
2. Docker版本：19.03.8 [官方下载地址](https://download.docker.com/linux/static/stable/x86_64/)

二、 Docker安装

1. 解压 `tar -xvf docker-18.06.1-ce.tgz`
2. 将解压出来的docker文件内容复制到 /usr/bin/ 目录下 `cp docker/* /usr/bin/`
3. 将docker注册为service `vim /etc/systemd/system/docker.service`，将下列配置加到docker.service中并保存

```text
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker

ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID

# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.

LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity

# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity

TimeoutStartSec=0

# set delegate yes so that systemd does not reset the cgroups of docker containers

Delegate=yes

# kill only the docker process, not all processes in the cgroup

KillMode=process

# restart the docker process if it exits prematurely

Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target
```

三、 启动

1. 添加文件执行权限 `chmod +x /etc/systemd/system/docker.service`  
2. 重载服务配置文件 `systemctl daemon-reload`  
3. 启动Docker `systemctl start docker`  
4. 设置开机自启 `systemctl enable docker.service`  

四、 验证

1. 查看/Docker状态 `systemctl status docker`  
2. 查看Docker版本 `docker -v`  


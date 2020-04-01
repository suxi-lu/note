# docker
学习笔记

* [centos](centos.md) docker 安装 centos
* [ssl](ssl.md) docker ssl访问
* [offline-install](offline-install.md) docker 离线安装
* [deploy](deploy) service 学习笔记

1. 普通用户执行docker  
<pre><code>
$ groupadd docker
$ gpasswd -a ${USER} docker
$ chmod a+rw /var/run/docker.sock
</code></pre>

2. 磁盘挂载权限  
<pre><code>
$ groupadd docker
$ gpasswd -a ${USER} docker
$ chmod a+rw /var/run/docker.sock
</code></pre>

3. tab命令自动补全
<pre><code>
$ yum install bash-completion
</code></pre>

4. 删除名称为none的镜像
<pre><code>
$ docker rmi $(docker images -f "dangling=true" -q)
</code></pre>

5. 删除Exited状态的容器
<pre><code>
$ docker rm $(sudo docker ps -qf status=exited)
</code></pre>
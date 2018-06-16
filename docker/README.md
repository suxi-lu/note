# docker
学习笔记

* [deploy](deploy) service 学习笔记

1.    普通用户执行docker  
<pre><code>
$ groupadd docker
$ gpasswd -a ${USER} docker
$ chmod a+rw /var/run/docker.sock
</code></pre>

2.    磁盘挂载权限  
<pre><code>
$ groupadd docker
$ gpasswd -a ${USER} docker
$ chmod a+rw /var/run/docker.sock
</code></pre>
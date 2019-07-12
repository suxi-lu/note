# centos 布署并添加ssh远程访问

#### 使用Docker下载CentOS镜像
* docker pull centos  

#### Docker启动CentOS镜像
* docker run --privileged --name=hdfs1 --network=bridge -p 10122:22 -p 9870:9870 -p 9864:9864 -dit -m 2GB centos /usr/sbin/init

#### 进入CentOS镜像容器
* docker exec -it hdfs1 bash  

* curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo  
* yum clean all  
* yum makecache  

* yum install openssh-clients  
* yum install openssh-server  

#### 修改ssh配置,允许root登录
* vi /etc/ssh/sshd_config  
```
# 将文件中，关于监听端口、监听地址前的 # 号去除
Port 22
#AddressFamily any
ListenAddress 0.0.0.0
ListenAddress ::

# 开启允许远程登录
PermitRootLogin yes

# 开启使用用户名密码来作为连接验证
PubkeyAuthentication yes  
```

#### 修改root用户密码
* passwd root  
输入用户密码

#### 开启ssh服务
* yum install net-tools  
 
* systemctl start sshd.service  
<pre><code>
输入命令 ps -e | grep sshd 检查是否开启了ssh服务 或 输入netstat -an | grep 22 检查 22 号端口是否开启监听  
</code></pre>

如果出现 service: command not found 则先安装service  
<pre><code>
输入命令 yum list | grep initscripts 查看版本  
输入命令 yum install initscripts -y 安装service  
输入命令 service sshd start 或 systemctl start sshd.service 开启ssh服务  
输入命令 ps -e | grep sshd 检查是否开启了ssh服务  
</code></pre>

启动sshd服务命令 
* systemctl start sshd.service  
重启 sshd服务命令 
* systemctl restart sshd.service  
设置服务开启自启命令 
* systemctl enable sshd.service  

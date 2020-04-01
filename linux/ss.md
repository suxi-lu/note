# ss
和谐上网

* 首先准备一台香港服务器
* 安装服务，这里以centos为例（https://github.com/shadowsocks/shadowsocks/wiki）  
<pre><code>
$ yum install python-setuptools && easy_install pip
$ pip install shadowsocks
</code></pre>
* 添加配置文件/etc/shadowsocks.json
<pre><code>
{
    "server":"my_server_ip", # 0.0.0.0
    "server_port":8388, # 服务端口
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword", # 密码
    "timeout":300,
    "method":"aes-256-cfb", # 加密方式
    "fast_open": false
}
</code></pre>
* 测试是否可以成功启动<code>ssserver -c /etc/shadowsocks.json</code>
* 后台运行
<pre><code>
$ ssserver -c /etc/shadowsocks.json -d start
$ ssserver -c /etc/shadowsocks.json -d stop
</code></pre>
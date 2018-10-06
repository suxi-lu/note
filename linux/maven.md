# maven

#### 环境变量配置
1.    修改环境变量配置  
<pre><code>
$ vi /etc/profile
</code></pre>

2.    添加配置  
<pre><code>
# STRAT
export MAVEN_HOME=/usr/java/apache-maven-3.5.3
export PATH=${PATH}:${MAVEN_HOME}/bin
# END
</code></pre>

3.    刷新环境变量配置  
<pre><code>
$ source /etc/profile
</code></pre>

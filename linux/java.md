# java

#### 环境变量配置
1.    修改环境变量配置  
<pre><code>
$ vi /etc/profile
</code></pre>

2.    添加配置  
<pre><code>
# STRAT
export JAVA_HOME=/usr/java/jdk1.8.0_171
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
# END
</code></pre>

3.    刷新环境变量配置  
<pre><code>
$ source /etc/profile
</code></pre>

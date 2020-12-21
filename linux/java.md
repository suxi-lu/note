# java

## 环境变量配置

* 修改环境变量配置

  ```text

  $ vi /etc/profile
  ```

* 添加配置

  ```text

  # STRAT
  export JAVA_HOME=/usr/java/jdk1.8.0_171
  export JRE_HOME=$JAVA_HOME/jre
  export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
  export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
  # END
  ```

* 刷新环境变量配置

  ```text

  $ source /etc/profile
  ```


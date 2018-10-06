# maven

#### 环境变量配置
* 修改环境变量配置  
<pre><code>
$ vi /etc/profile
</code></pre>

* 添加配置  
<pre><code>
# STRAT
export MAVEN_HOME=/usr/java/apache-maven-3.5.3
export PATH=${PATH}:${MAVEN_HOME}/bin
# END
</code></pre>

* 刷新环境变量配置  
<pre><code>
$ source /etc/profile
</code></pre>

#### docker plugin 配置
* properties
```
    <docker-maven-plugin.version>1.1.1</docker-maven-plugin.version>
    <baseImage>java:8</baseImage>
    <dockerHost>http://192.168.80.130:2375</dockerHost>
```

* plugin
```
    <plugin>
        <groupId>com.spotify</groupId>
        <artifactId>docker-maven-plugin</artifactId>
        <version>${docker-maven-plugin.version}</version>
        <configuration>
            <imageName>${project.groupId}/${project.artifactId}:${project.version}</imageName>  <!--指定镜像名称 仓库/镜像名:标签-->
            <dockerDirectory>${project.basedir}/src/main/docker</dockerDirectory>
            <baseImage>${baseImage}</baseImage>    <!--指定基础镜像，相当于dockerFile中的FROM<image> -->
            <dockerHost>${dockerHost}</dockerHost>  <!-- 指定仓库地址 -->
            <forceTags>true</forceTags>    <!--覆盖相同标签镜像-->
            <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint> <!-- 容器启动执行的命令，相当于dockerFile的ENTRYPOINT -->
            <!-- copy the service's jar file from target into the root directory of the image -->
            <resources>
                <resource>                        <!-- 指定资源文件 -->
                    <targetPath>/</targetPath>            <!-- 指定要复制的目录路径，这里是当前目录 -->
                    <directory>${project.build.directory}</directory> <!-- 指定要复制的根目录，这里是target目录 -->
                    <include>${project.build.finalName}.jar</include>  <!-- 指定需要拷贝的文件，这里指最后生成的jar包 -->
                </resource>
            </resources>
        </configuration>
    </plugin>
```

* 构建命令
```
mvn -DskipTests clean package docker:build
```

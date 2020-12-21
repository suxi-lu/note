# docker

学习笔记

1. [deploy 学习笔记](deploy/)
2. [centos](centos.md)
3. [docker 远程访问](remote.md)
4. [tls 配置远程api tls访问](tls.md)
5. [docker 离线安装](offline-install.md)
6. 普通用户执行docker

   ```text

   $ groupadd docker
   $ gpasswd -a ${USER} docker
   $ chmod a+rw /var/run/docker.sock
   ```

7. 磁盘挂载权限

   ```text

   $ groupadd docker
   $ gpasswd -a ${USER} docker
   $ chmod a+rw /var/run/docker.sock
   ```

8. tab命令自动补全

   ```text

   $ yum install bash-completion
   ```

9. 删除名称为none的镜像

   ```text

   $ docker rmi $(docker images -f "dangling=true" -q)
   ```

10. 删除Exited状态的容器

    ```text

    $ docker rm $(sudo docker ps -qf status=exited)
    ```


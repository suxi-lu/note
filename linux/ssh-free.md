# ssh

远程主机免密登录

* 执行下面命令

  ```text

  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
  ```

* 切换目录

  ```text

  cd ~/.ssh/
  ```

* 下载公钥

  ```text

  sz id_rsa.pub
  ```

* secure CRT 免密登录配置

  ```text

  Properties -> Connection -> SSH2  
  -> Authentication 选中 PublicKey 点击 Properties...
  选择 Use session public key setting 点击 Use identify or certificate file 选择下载下来的公钥
  登录
  ```


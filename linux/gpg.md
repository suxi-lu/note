# gpg 密钥生成

## 生成步骤

* 生成密钥对

  ```text
  $ gpg --gen-key
  ```

* 列表键

  ```text
  $ gpg --list-keys
  ```

* 上传公钥

  ```text
  $ gpg --keyserver hkps://keys.openpgp.org --send-keys <key>
  ```

* 格式化 key 为 long 类型

  ```text
  $ gpg --list-secret-keys --keyid-format LONG <email>
  ```

* 格式化 key 为 short 类型

  ```text
  $ gpg --list-secret-keys --keyid-format SHORT <email>
  ```

* 导出 secret key 到 gpg 文件

  ```text
  $ gpg --export-secret-key <long_key> ~/.gnupg/secring.gpg
  ```


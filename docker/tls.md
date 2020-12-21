# tls

## docker 配置 tls 远程访问

## 使用OpenSSL创建CA，服务器和客户端密钥

`注意：将$HOST以下示例中的所有实例替换为Docker守护程序主机的DNS名称。`

#### 首先，在Docker守护程序的主机上，生成CA私钥和公钥：

```text

# openssl genrsa -aes256 -out ca-key.pem 4096
Generating RSA private key, 4096 bit long modulus
............................................................................................................................................................................................++
........++
e is 65537 (0x10001)
Enter pass phrase for ca-key.pem:
Verifying - Enter pass phrase for ca-key.pem:

# openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
Enter pass phrase for ca-key.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:CN
Locality Name (eg, city) [Default City]:SH
Organization Name (eg, company) [Default Company Ltd]:CN
Organizational Unit Name (eg, section) []:CN
Common Name (eg, your name or your server's hostname) []:$HOST
Email Address []:
```

#### 现在您已经有了CA，您可以创建服务器密钥和证书签名请求（CSR）。确保“公用名”与您用于连接Docker的主机名匹配：

`注意：将$HOST以下示例中的所有实例替换为Docker守护程序主机的DNS名称。`

```text

# openssl genrsa -out server-key.pem 4096
Generating RSA private key, 4096 bit long modulus
.....................................................................++
.................................................................................................++
e is 65537 (0x10001)

# openssl req -subj "/CN=$HOST" -sha256 -new -key server-key.pem -out server.csr
```

接下来，我们将用CA签署公钥：

由于可以通过IP地址和DNS名称建立TLS连接，因此在创建证书时需要指定IP地址。例如，允许使用10.10.10.20和127.0.0.1进行连接：

```text

# cat /etc/resolv.conf

# echo subjectAltName = DNS:$HOST,IP:192.168.89.128,IP:127.0.0.1 >> extfile.cnf
```

将Docker守护程序密钥的扩展用法属性设置为仅用于服务器身份验证：

```text

# echo extendedKeyUsage = serverAuth >> extfile.cnf
```

现在，生成签名证书：

```text

# openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem \
  -CAcreateserial -out server-cert.pem -extfile extfile.cnf
```

授权插件提供了更细粒度的控制，以补充来自相互TLS的身份验证。除了上述文档中描述的其他信息外，在Docker守护程序上运行的授权插件还会接收用于连接Docker客户端的证书信息。

对于客户端身份验证，创建客户端密钥和证书签名请求： `注意：为了简化接下来的几个步骤，您也可以在Docker守护程序的主机上执行此步骤。`

```text

# openssl genrsa -out key.pem 4096
Generating RSA private key, 4096 bit long modulus
.........................................................++
................++
e is 65537 (0x10001)

# openssl req -subj '/CN=client' -new -key key.pem -out client.csr
```

为了使密钥适合客户端身份验证，请创建一个新的扩展配置文件：

```text

# echo extendedKeyUsage = clientAuth > extfile-client.cnf
```

现在，生成签名证书：

```text

# openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem \
    -CAcreateserial -out cert.pem -extfile extfile-client.cnf
Signature ok
subject=/CN=client
Getting CA Private Key
Enter pass phrase for ca-key.pem:
```

生成后`cert.pem`，`server-cert.pem` 您可以安全地删除两个证书签名请求和扩展配置文件：

```text

# rm -v client.csr server.csr extfile.cnf extfile-client.cnf
```

默认 `umask` 密钥为022，您的密钥对您和您的组是全球可读的且可写的。

为了保护您的钥匙免遭意外损坏，请删除其写权限。要使它们仅供您阅读，请按以下方式更改文件模式：

```text

# chmod -v 0400 ca-key.pem key.pem server-key.pem
```

证书可以在世界范围内读取，但您可能希望删除写访问权限以防止意外损坏：

```text

# chmod -v 0444 ca.pem server-cert.pem cert.pem
```

现在，您可以使Docker守护程序仅接受来自提供CA信任的证书的客户端的连接：

```text

# dockerd --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem \
  -H=0.0.0.0:2376
```

要连接到Docker并验证其证书，请提供您的客户端密钥，证书和受信任的CA：

```text
在客户端计算机上运行

此步骤应在Docker客户端计算机上运行。这样，您需要将CA证书，服务器证书和客户端证书复制到该计算机上。
```

`注意：将$HOST以下示例中的所有实例替换为Docker守护程序主机的DNS名称。`

```text

# docker --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem \
  -H=$HOST:2376 version
```

`注意：基于TLS的Docker应该在TCP端口2376上运行。`

```text
警告：如上例所示，使用证书身份验证时，您不需要docker使用sudo或docker组运行客户端。这意味着拥有密钥的任何人都可
以向您的Docker守护程序提供任何指令，从而赋予他们对托管该守护程序的机器的root访问权限。像使用root密码一样保护这些密钥！
```

### 默认安全

如果要默认保护Docker客户端连接的安全性，可以将文件移动到`.docker`主目录--- 的目录中，并同时设置 `DOCKER_HOST`和 `DOCKER_TLS_VERIFY`变量（而不是在每次调用时传递 `-H=tcp://$HOST:2376`和`--tlsverify`）。

```text

# mkdir -pv ~/.docker
# cp -v {ca,cert,key}.pem ~/.docker
# cp -v {server-cert,server-key}.pem ~/.docker

# export DOCKER_HOST=tcp://$HOST:2376 DOCKER_TLS_VERIFY=1
```

Docker现在默认情况下安全连接：

```text

# docker ps
```

注： 修改docker service 文件  
`# vi vi /usr/lib/systemd/system/docker.service`  
修改 ExecStart 如下（参考）：

```text
ExecStart=/usr/bin/dockerd -H unix://var/run/docker.sock -H tcp://$HOST:2376 --tlsverify --tlscacert=/root/.docker/ca.pem --tlscert=/root/.docker/server-cert.pem --tlskey=/root/.docker/server-key.pem
```

重启docker  
`systemctl daemon-reload`  
`systemctl restart docker`

### 使用连接到安全的Docker端口 `curl`

要用于 `curl` 发出测试API请求，您需要使用三个额外的命令行标志：

```text

# curl https://$HOST:2376/images/json \
  --cert ~/.docker/cert.pem \
  --key ~/.docker/key.pem \
  --cacert ~/.docker/ca.pem
```


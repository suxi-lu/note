# mysql 授权

_创建数据库_

```text

create database auth;
```

_创建用户_

```text

CREATE USER 'learn'@'%' IDENTIFIED BY 'learn';
```

_修改用户密码_

```text

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new password';
```

_禁用root用户远程访问权限_

```text

use mysql;
update user set host = "127.0.0.1" where user = "root" and host = "%";
flush privileges;
```

_启用root用户远程访问权限_

```text

use mysql;
update user set host = "%" where user = "root" and host = "127.0.0.1";
flush privileges;
```

_授权数据库权限_

```text

GRANT ALL ON auth.* TO 'learn'@'%' WITH GRANT OPTION;
```

_刷新权限_

```text

flush privileges;
```


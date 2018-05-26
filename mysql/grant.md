# mysql 授权  

*创建数据库*
<pre><code>
create database auth;
</code></pre>

*创建用户*  
<pre><code>
CREATE USER 'learn'@'%' IDENTIFIED BY 'learn';
</code></pre>

*修改用户密码*
<pre><code>
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new password';
</code></pre>

*禁用root用户远程访问权限*
<pre><code>
use mysql;
update user set host = "127.0.0.1" where user = "root" and host = "%";
flush privileges;
</code></pre>

*启用root用户远程访问权限*
<pre><code>
use mysql;
update user set host = "%" where user = "root" and host = "127.0.0.1";
flush privileges;
</code></pre>

*授权数据库权限*
<pre><code>
GRANT ALL ON auth.* TO 'learn'@'%' WITH GRANT OPTION;
</code></pre>

*刷新权限*
<pre><code>
flush privileges;
</code></pre>
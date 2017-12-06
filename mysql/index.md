# 开启远程连接

- 设置连接用户

```
mysql -u root -p mysql


grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
#root是用户名，%代表任意主机，'123456'指定的登录密码（这个和本地的root密码可以设置不同的，互不影响）

flush privileges;
# 重载系统权限

exit;


```

- 允许3360端口

```
iptables -I INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

# 查看规则是否生效
iptables -L -n # 或者: service iptables status

# 此时生产环境是不安全的，远程管理之后应该关闭端口，删除之前添加的规则
iptables -D INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

上面iptables添加/删除规则都是临时的，如果需要重启后也生效，需要保存修改:
service iptables save # 或者: /etc/init.d/iptables save

vi /etc/sysconfig/iptables # 加上下面这行规则也是可以的
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

```

# 启动&&停止

- 数据库字符集设置

    `mysql配置文件/etc/my.cnf中加入default-character-set=utf8`

- 启动mysql服务：

    `service mysqld start或者/etc/init.d/mysqld start`

- 开机启动：

    ```
    chkconfig -add mysqld，查看开机启动设置是否成功chkconfig --list | grep mysql*
    mysqld             0:关闭    1:关闭    2:启用    3:启用    4:启用    5:启用    6:关闭
    ```

- 停止：

    `service mysqld stop`


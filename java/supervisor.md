# supervisor 教程

## 官网 <http://supervisord.org/running.html>

## 教程

- <https://blog.csdn.net/qq_27754983/article/details/78782866>

/etc/supervisord.d/wallet-api.ini
类似这个配置应该就可以了，其中[program:wallet-api]程序名
command 执行的 jar 启动命令
如果你 logback 的路径改成 <property name="LOG_HOME" value="/data/log/wallet/xxx"></property> 这样的话，那 command 里面的-Dlogging.config 跟 -Dlogback.configurationFile 可以不用写
这个是配置
supervisorctl 进入管理控制台
reread 这个重新读取配置，新添加的启动进程配置能读进来，update 更新配置
start/stop/restart xxx 启动/停止/重启 xxx 进程

/data/wwwroot/wallet.putaotec.com
supervisorctl
restart xxxxxxx

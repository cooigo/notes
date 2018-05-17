# java web 手机端接口调试（window 环境）

1.安装 nginx, 下载地址: <http://nginx.org/en/download.html>

2.配置 nginx 反代,在安装的目录找 nginx.conf 文件，例如：`C:\nginx-1.14.0\conf\nginx.conf`,在 http 节点下增加 api 接口反代

```conf
server {
       listen       api.xxx.com:80;
       server_name  api.xxx.com;

       location / {
           proxy_pass http://localhost:8080; //本地调试环境，测试地址
       }
    }
```

3.增本地 host，127.0.0.1 api.xxx.com

4.禁用其他使用 80 端的程序，例如：在 window 环境，先关闭 IIS 服务

5.开启 nginx 服务, 在安装目录下 直接点击 nginx.exe

6.如果启用成功可以在 window 进程 查看到 nginx.exe 进程，或者使用命令 `tasklist /fi "imagename eq nginx.exe"`

7.更多可查看官方文档<http://nginx.org/en/docs/windows.html>

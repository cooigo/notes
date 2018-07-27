# Window docker 使用

## 查看本机镜像

```
docker images
```

## 查看正在运行的容器

```
docker ps -a
```

## 删除所有的容器

```
docker rm [name/id,]
```

## 停止、启动、杀死一个容器

```
docker stop Name/ID 
docker start Name/ID 
docker kill Name/ID
```

## 交互式进入容器中

```
docker run -i -t image_name /bin/bash
```

## 在容器中安装新的程序

```
docker run image_name apt-get install -y app_name
```

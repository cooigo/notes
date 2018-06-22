# Linux 开发环境

## jdk 安装

1.  在/usr 目录中先建名为 java 的文件夹

```
mkdir /usr/java
```

2.  下载 jdk-8u171-linux-x64.tar.gz 包<http://www.oracle.com/technetwork/java/javase/downloads/index.html>，并上传至服务器/usr/local 文件夹中。

3.  解压 jdk-8u171-linux-x64.tar.gz 包至/usr/local/jdk1.8.0_171 文件夹

```
tar -zxvf jdk-8u171-linux-x64.tar.gz
```

4.  添加到环境变量

- 编辑/etc/profile 文件，在 export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL 下面添加如下代码：

```
#jdk
JAVA_HOME=/usr/local/jdk1.8.0_171
JRE_HOME=$JAVA_HOME/jre
PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH
```

- 执行命令使配置生效

```
source /etc/profile
```

5.  验证，是否安装成功

```
java -version
```

## maven 安装

1.  下载 apache-maven-3.5.3-bin.tar.gz 包 <http://maven.apache.org/download.cgi> 并上传至服务器/usr/local 文件夹中。

2.  解压 apache-maven-3.5.3-bin.tar.gz

```
tar -zxvf apache-maven-3.5.3-bin.tar.gz
```

3.  添加到环境变量

- 编辑/etc/profile 文件，在 export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL 下面添加如下代码：

```
MAVEN_HOME=/usr/local/apache-maven-3.5.3
PATH=$MAVEN_HOME:$PATH
export MAVEN_HOME PATH
```

- 执行命令使配置生效

```
source /etc/profile
```

4.  验证，是否安装成功

```
mvn -v
```

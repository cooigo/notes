# 技术点

## 数据库连接池 alibaba druid

- <https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter>

## 前端框架

- UI vue-element-admin <https://github.com/PanJiaChen/vue-element-admin/blob/master/README.zh-CN.md>

## 安全框架 Apache Shiro

- 《跟我学 Shiro》 <http://jinnianshilongnian.iteye.com/blog/2049092>

## 持久层框架 MyBatis MyBatis-Plus

- <http://mp.baomidou.com/guide>

## 日志管理 SLF4J Log4j

## 开源框架

- <https://www.renren.io/>

## json 序列化 Gson

- https://github.com/google/gson/blob/master/UserGuide.md#TOC-Overview

## 常用数据连接配置

```
 # Properties file with JDBC-related settings.
##########
# HSQLDB #
##########
#jdbc.driverClassName=org.hsqldb.jdbcDriver
#jdbc.url=jdbc:hsqldb:hsql://localhost:9001/bookstore
#jdbc.username=sa
#jdbc.password=
###########
# MySQL 5 #
###########
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=GBK
jdbc.username=root
jdbc.password=root
##############
# PostgreSQL #
##############
#jdbc.driverClassName=org.postgresql.Driver
#jdbc.url=jdbc:postgresql://localhost/bookstore
#jdbc.username=
#jdbc.password=
##########
# Oracle #
##########
#jdbc.driverClassName=oracle.jdbc.driver.OracleDriver
#jdbc.url=jdbc:oracle:thin:@192.168.1.250:1521:devdb
#jdbc.username=HFOSPSP
#jdbc.password=HFOSPSP
#############################
# MS SQL Server 2000 (JTDS) #
#############################
#jdbc.driverClassName=net.sourceforge.jtds.jdbc.Driver
#jdbc.url=jdbc:jtds:sqlserver://localhost:1433/bookstore
#jdbc.username=
#jdbc.password=
##################################
# MS SQL Server 2000 (Microsoft) #
##################################
#jdbc.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver
#jdbc.url=jdbc:sqlserver://192.168.1.130:1433;database=ahos;user=sa;password=ahtec";
#jdbc.username=sa
#jdbc.password=ahtec
########
# ODBC #
########
#jdbc.driverClassName=sun.jdbc.odbc.JdbcOdbcDriver
#jdbc.url=jdbc:odbc:bookstore
#jdbc.username=
#jdbc.password=
```

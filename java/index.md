# spring boot 计划任务

* <https://spring.io/guides/gs/scheduling-tasks/#initial>

# http 请求

经常需要发送一个 GET/POST 请求到其他系统(REST API)，通过 JDK 自带的 HttpURLConnection、Apache HttpClient、Netty 4、OkHTTP 2/3 都可以实现。

* <https://docs.spring.io/spring/docs/5.0.5.RELEASE/spring-framework-reference/web.html#spring-web>

* <https://docs.spring.io/spring/docs/5.0.5.RELEASE/spring-framework-reference/integration.html#rest-client-access>

* <http://rensanning.iteye.com/blog/2362105>

# 表单验证

限制 说明

* @Null 限制只能为 null
* @NotNull 限制必须不为 null
* @AssertFalse 限制必须为 false
* @AssertTrue 限制必须为 true
* @DecimalMax(value) 限制必须为一个不大于指定值的数字
* @DecimalMin(value) 限制必须为一个不小于指定值的数字
* @Digits(integer,fraction) 限制必须为一个小数，且整数部分的位数不能超过 integer，小数部分的位数不能超过 fraction
* @Future 限制必须是一个将来的日期
* @Max(value) 限制必须为一个不大于指定值的数字
* @Min(value) 限制必须为一个不小于指定值的数字
* @Past 限制必须是一个过去的日期
* @Pattern(value) 限制必须符合指定的正则表达式
* @Size(max,min) 限制字符长度必须在 min 到 max 之间
* @Past 验证注解的元素值（日期类型）比当前时间早
* @NotEmpty 验证注解的元素值不为 null 且不为空（字符串长度不为 0、集合大小不为 0）
* @NotBlank 验证注解的元素值不为空（不为 null、去除首位空格后长度为 0），不同于@NotEmpty，@NotBlank 只应用于字符串且在比较时会去除字符串的空格
* @Email 验证注解的元素值是 Email，也可以通过正则表达式和 flag 指定自定义的 email 格式

# 常用工具类

* http 命令行工具 <https://httpie.org/>

* <http://commons.apache.org/>

* <https://commons.apache.org/proper/commons-lang/dependency-info.html>

* <https://commons.apache.org/proper/commons-beanutils/dependency-info.html>

* <https://github.com/google/guava> <http://ifeve.com/google-guava/>

* <https://www.malot.fr/bootstrap-datetimepicker/>

* <https://www.jianshu.com/p/d43d9acf949f> 在 idea 中我们可以设置如下操作，自动生成 serialVersionUID, File->Setting->Editor->Inspections->Serialization issues->Serializable class without ’serialVersionUID’ ->勾选操作

# Thymeleaf 教程

* 文档 <http://www.thymeleaf.org/documentation.html>

* 布局 <http://www.thymeleaf.org/doc/articles/layouts.html>

* 语法 <http://www.thymeleaf.org/doc/articles/standarddialect5minutes.html>

```html
<!DOCTYPE html>
<html lang="en" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

# Spring Boot 教程

* 文档 <https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle>

* 源码 <https://github.com/spring-projects/spring-boot>

* 第三方教程 <https://www.zhihu.com/question/53729800>

* <http://www.vxzsk.com/251.html>

* Spring boot 中使用 Shiro <https://www.jianshu.com/p/0f2049a3983b>

* Spring boot 配置 <http://blog.didispace.com/springbootproperties/>

* Spring Boot 中使用 AOP 统一处理 Web 请求日志 <http://blog.didispace.com/springbootaoplog/>

# WEB 建站资料

## 开始建项目

* <https://start.spring.io/>

* idea 编辑器 File -> New -> Project -> Spring Initializr

## 添加依赖

### web 参考资料

* <http://www.baeldung.com/>

* <https://spring.io/guides/gs/serving-web-content/>

* <https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-controller>

* <https://medium.com/@gustavo.ponce.ch/spring-boot-spring-mvc-spring-security-mysql-a5d8545d837d>

* 属性配置 <https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-external-config-typesafe-configuration-properties>

* vaadin 自定义样式 <https://www.sothawo.com/2015/06/custom-theme-for-a-vaadin-spring-boot-application/>

* json 输出 <https://diamondfsd.com/article/ae89cf4e-f679-4cc0-9882-f02e0240866e>

* <https://github.com/ityouknow/spring-boot-examples>

### security

* <https://spring.io/guides/gs/securing-web/>

* <https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-security>

* <https://docs.spring.io/spring-security/site/docs/4.2.3.RELEASE/reference/htmlsingle/#jc-method>

* <http://blog.csdn.net/bestcxx/article/details/78048385>

* 用户密码加密 <http://www.jianshu.com/p/a3f85f350a33>

* springboot 整合 shiro-登录认证和权限管理 <http://www.ityouknow.com/springboot/2017/06/26/springboot-shiro.html>

* shiro <http://jinnianshilongnian.iteye.com/blog/2018398> <https://github.com/apache/shiro> <https://shiro.apache.org/spring-boot.html>

* <http://412887952-qq-com.iteye.com/blog/2299777>

* Spring MVC 防止数据重复提交 <http://blog.icoolxue.com/submitted-by-spring-mvc-to-prevent-data-duplication/>

* <https://qtdebug.com/spring-web-form-resubmit/>

### 统一异常处理

* <https://www.jianshu.com/p/3998ea8b53a8>

* <http://blog.didispace.com/springbootexception/>

* <http://blog.csdn.net/songjinbin/article/details/14003533>

* SpringBoot 基础教程 <https://www.jianshu.com/p/964370d9374e>

### jpa

* <https://docs.spring.io/spring-data/jpa/docs/2.0.2.RELEASE/reference/html/>

* 自定义实现 <https://docs.spring.io/spring-data/jpa/docs/2.0.2.RELEASE/reference/html/#repositories.custom-implementations>

* 参数查询 <https://docs.spring.io/spring-data/jpa/docs/2.0.2.RELEASE/reference/html/#jpa.named-parameters>

* 审计 <http://juhahinkula.github.io/2017-04-10-jpaauditing/>

### mysql

* <https://spring.io/guides/gs/accessing-data-mysql/>

### thymeleaf

* <http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html>

* <http://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html>

* <https://ultraq.github.io/thymeleaf-layout-dialect/Examples.html>

* 安全 <https://memorynotfound.com/spring-boot-spring-security-thymeleaf-form-login-example/>

* <https://github.com/theborakompanioni/thymeleaf-extras-shiro>

```
//pom.xml

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
    <thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>
    <thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>
    <thymeleaf-extras-springsecurity4.version>3.0.2.RELEASE</thymeleaf-extras-springsecurity4.version>
</properties>
<dependencies>
    <dependency>
        <groupId>org.thymeleaf.extras</groupId>
        <artifactId>thymeleaf-extras-springsecurity4</artifactId>
    </dependency>
</dependencies>

//html
<html xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4">
```

```
4 Standard Expression Syntax

Simple expressions:
Variable Expressions: ${...}
Selection Variable Expressions: *{...}
Message Expressions: #{...}
Link URL Expressions: @{...}
Fragment Expressions: ~{...}

Literals
Text literals: 'one text', 'Another one!',…
Number literals: 0, 34, 3.0, 12.3,…
Boolean literals: true, false
Null literal: null
Literal tokens: one, sometext, main,…

Text operations:
String concatenation: +
Literal substitutions: |The name is ${name}|

Arithmetic operations:
Binary operators: +, -, *, /, %
Minus sign (unary operator): -

Boolean operations:
Binary operators: and, or
Boolean negation (unary operator): !, not

Comparisons and equality:
Comparators: >, <, >=, <= (gt, lt, ge, le)
Equality operators: ==, != (eq, ne)

Conditional operators:
If-then: (if) ? (then)
If-then-else: (if) ? (then) : (else)
Default: (value) ?: (defaultvalue)
Special tokens:

No-Operation: _
```

### autuator

* <https://spring.io/guides/gs/actuator-service/>

### devtools

* <https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#using-boot-devtools>

* 热更新 <http://tengj.top/2017/06/01/springboot10/>

## 其他参考资料

* <https://www.callicoder.com/spring-boot-rest-api-tutorial-with-mysql-jpa-hibernate/>

## vaadin

* <https://demo.vaadin.com/sampler/>

* <http://vaadin.github.io/spring-tutorial/>

* <https://vaadin.com/api/vaadin-spring/>

* <https://spring.io/guides/gs/crud-with-vaadin/>

* 数据分页 <https://github.com/Artur-/spring-data-provider>

* 数据过滤 <https://vaadin.com/docs/v8/framework/datamodel/datamodel-providers.html>

* icon <https://vaadin.com/elements/vaadin-icons/html-examples/icons-basic-demos>

* 样式 <https://github.com/vaadin/valo-demo>

* 主题 doc <https://vaadin.com/docs/v8/framework/themes/themes-valo.html>

- <https://bakery.demo.vaadin.com/#!user-admin>

- <https://demo.vaadin.com>

- <https://vaadin.com/api/>

- <https://vaadin.com/docs/v8/framework/introduction/intro-overview.html>

# window 查询端口调用

引用地址：<http://blog.51cto.com/58582786/671487>

当然前提是 cmd.exe 是以管理员身份执行的，不然会报“拒绝 xx”的错.....

1、netstat -ano |findstr 8088 //查看 8088 端口是否存在 记下最后一位数字，即 PID

2、tasklist |findstr 10152（PID 号）//查看 pid 为 10152 的是什么程序在用

3、taskkill /T /F /PID 10152 //强制（/F 参数）杀死 pid 为 10152 的所有进程包括子进程（/T 参数）

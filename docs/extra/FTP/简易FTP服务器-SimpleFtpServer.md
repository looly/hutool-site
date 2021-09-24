## 介绍

Hutool基于 [Apache FtpServer]（http://mina.apache.org/ftpserver-project/）封装了一个简易的FTP服务端组件，主要用于在一些测试场景或小并发应用场景下使用。

## 使用

### 引入FtpServer

```xml
<dependency>
	<groupId>org.apache.ftpserver</groupId>
	<artifactId>ftpserver-core</artifactId>
	<version>1.1.1</version>
</dependency>
```

### 使用

- 开启匿名FTP服务：

```java
SimpleFtpServer
	.create()
	// 此目录必须存在
	.addAnonymous("d:/test/ftp/")
	.start();
```

此时就可以通过资源管理器访问：

```
ftp://localhost
```

- 自定义用户

```java
BaseUser user = new BaseUser();
user.setName("username");
user.setPassword("123");
user.setHomeDirectory("d:/test/user/");

SimpleFtpServer
	.create()
	.addUser(user)
	.start();
```
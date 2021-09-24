## 介绍

FTP客户端封装，此客户端基于[Apache Commons Net](http://commons.apache.org/proper/commons-net/)。

## 使用

### 引入依赖

```xml
<dependency>
	<groupId>commons-net</groupId>
	<artifactId>commons-net</artifactId>
	<version>3.6</version>
</dependency>
```

### 使用

```java
//匿名登录（无需帐号密码的FTP服务器）
Ftp ftp = new Ftp("172.0.0.1");
//进入远程目录
ftp.cd("/opt/upload");
//上传本地文件
ftp.upload("/opt/upload", FileUtil.file("e:/test.jpg"));
//下载远程文件
ftp.download("/opt/upload", "test.jpg", FileUtil.file("e:/test2.jpg"));

//关闭连接
ftp.close();
```

### 主动模式与被动模式

- PORT（主动模式）

> FTP客户端连接到FTP服务器的21端口，发送用户名和密码登录，登录成功后要list列表或者读取数据时，客户端随机开放一个端口（1024以上），发送 PORT命令到FTP服务器，告诉服务器客户端采用主动模式并开放端口；FTP服务器收到PORT主动模式命令和端口号后，通过服务器的20端口和客户端开放的端口连接，发送数据。

- PASV（被动模式）

> FTP客户端连接到FTP服务器的21端口，发送用户名和密码登录，登录成功后要list列表或者读取数据时，发送PASV命令到FTP服务器， 服务器在本地随机开放一个端口（1024以上），然后把开放的端口告诉客户端， 客户端再连接到服务器开放的端口进行数据传输。

更多介绍见：https://www.cnblogs.com/huhaoshida/p/5412615.html

Ftp中默认是被动模式，需要切换则：

```java
Ftp ftp = new Ftp("172.0.0.1");

//切换为主动模式
ftp.setMode(FtpMode.Active);
```
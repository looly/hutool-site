## 介绍
本质上，HttpUtil中的get和post工具方法都是HttpRequest对象的封装，因此如果想更加灵活操作Http请求，可以使用HttpRequest。

## 使用

### 普通表单
我们以POST请求为例：

```java
//链式构建请求
String result2 = HttpRequest.post(url)
	.header(Header.USER_AGENT, "Hutool http")//头信息，多个头信息多次调用此方法即可
	.form(paramMap)//表单内容
	.timeout(20000)//超时，毫秒
	.execute().body();
Console.log(result2);
```

通过链式构建请求，我们可以很方便的指定Http头信息和表单信息，最后调用execute方法即可执行请求，返回HttpResponse对象。HttpResponse包含了服务器响应的一些信息，包括响应的内容和响应的头信息。通过调用body方法即可获取响应内容。

### Restful请求
```java
String json = ...;
String result2 = HttpRequest.post(url)
	.body(json)
	.execute().body();
```

### 配置代理

如果代理无需账号密码，可以直接：

```java
String result2 = HttpRequest.post(url)
	.setHttpProxy("127.0.0.1", 9080)
	.body(json)
	.execute().body();
```

如果需要自定其他类型代理或更多的项目，可以：

```java
String result2 = HttpRequest.post(url)
	.setProxy(new Proxy(Proxy.Type.HTTP,
				new InetSocketAddress(host, port))
	.body(json)
	.execute().body();
```

如果遇到https代理错误`Proxy returns "HTTP/1.0 407 Proxy Authentication Required"`，可以尝试：

```java
System.setProperty("jdk.http.auth.tunneling.disabledSchemes", "");
Authenticator.setDefault(
    new Authenticator() {
        @Override
        public PasswordAuthentication getPasswordAuthentication() {
              return new PasswordAuthentication(authUser, authPassword.toCharArray());
        }
    }
);
```

## 其它自定义项
同样，我们通过HttpRequest可以很方便的做以下操作：

- 指定请求头
- 自定义Cookie（cookie方法）
- 指定是否keepAlive（keepAlive方法）
- 指定表单内容（form方法）
- 指定请求内容，比如rest请求指定JSON请求体（body方法）
- 超时设置（timeout方法）
- 指定代理（setProxy方法）
- 指定SSL协议（setSSLProtocol）
- 简单验证（basicAuth方法）


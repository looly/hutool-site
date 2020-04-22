## 由来

Oracle JDK提供了一个简单的Http服务端类，叫做`HttpServer`，当然它是sun的私有包，位于com.sun.net.httpserver下，必须引入rt.jar才能使用，Hutool基于此封装了`SimpleServer`，用于在不引入Tomcat、Jetty等容器的情况下，实现简单的Http请求处理。

> SimpleServer在Hutool-5.3.0后才引入，请升级到最新版本

## 使用

1. 启动一个Http服务非常简单：

```java
HttpUtil.createServer(8888).start();
```
通过浏览器访问 [http://localhost:8888/](http://localhost:8888/) 即可，当然此时访问任何path都是404。

2. 处理简单请求：

```
HttpUtil.createServer(8888)
    .addAction("/", (req, res)->{
        res.write("Hello Hutool Server");
    })
    .start();
```

此处我们定义了一个简单的action，绑定在"/"路径下，此时我们可以访问，输出“Hello Hutool Server”。

同理，我们通过调用addAction方法，定义不同path的处理规则，实现相应的功能。

### 简单的文件服务器

Hutool默认提供了简单的文件服务，即定义一个root目录，则请求路径后直接访问目录下的资源，默认请求`index.html`，类似于Nginx。

```
HttpUtil.createServer(8888)
    // 设置默认根目录
    .setRoot("D:\\workspace\\site\\hutool-site")
    .start();
```

此时访问[http://localhost:8888/](http://localhost:8888/)即可访问HTML静态页面。

> hutool-site是Hutool主页的源码项目，地址在：[https://gitee.com/loolly_admin/hutool-site](https://gitee.com/loolly_admin/hutool-site)，下载后配合SimpleServer实现离线文档。

### 读取请求和返回内容

有时候我们需要自定义读取请求参数，然后根据参数访问不同的数据，整理返回，此时我们自定义Action即可完成：

1. 返回JSON数据

```java
HttpUtil.createServer(8888)
    // 返回JSON数据测试
    .addAction("/restTest", (request, response) ->
    		response.write("{\"id\": 1, \"msg\": \"OK\"}", ContentType.JSON.toString())
    ).start();
```

2. 获取表单数据并返回

```java
HttpUtil.createServer(8888)
    // http://localhost:8888/formTest?a=1&a=2&b=3
    .addAction("/formTest", (request, response) ->
        response.write(request.getParams().toString(), ContentType.TEXT_PLAIN.toString())
    ).start();
```

### 文件上传

除了常规Http服务，Hutool还封装了文件上传操作：

```java
HttpUtil.createServer(8888)
    .addAction("/file", (request, response) -> {
        final UploadFile file = request.getMultipart().getFile("file");
        // 传入目录，默认读取HTTP头中的文件名然后创建文件
        file.write("d:/test/");
        response.write("OK!", ContentType.TEXT_PLAIN.toString());
        }
    )
    .start();
```
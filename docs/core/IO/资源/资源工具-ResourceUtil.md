## 介绍

`ResourceUtil`提供了资源快捷读取封装。

## 使用

`ResourceUtil`中最核心的方法是`getResourceObj`，此方法可以根据传入路径是否为绝对路径而返回不同的实现。比如路径是：`file:/opt/test`，或者`/opt/test`都会被当作绝对路径，此时调用`FileResource`来读取数据。如果不满足以上条件，默认调用`ClassPathResource`读取classpath中的资源或者文件。

同样，此工具类还封装了`readBytes`和`readStr`用于快捷读取bytes和字符串。

举个例子，假设我们在classpath下放了一个`test.xml`，读取就变得非常简单：

```java
String str = ResourceUtil.readUtf8Str("test.xml");
```

假设我们的文件存放在`src/resources/config`目录下，则读取改为：

```java
String str = ResourceUtil.readUtf8Str("config/test.xml");
```

> 注意
> 在IDEA中，新加入文件到`src/resources`目录下，需要重新import项目，以便在编译时顺利把资源文件拷贝到target目录下。如果提示找不到文件，请去target目录下确认文件是否存在。
## 介绍

SPI（Service Provider Interface），是一种服务发现机制。它通过在ClassPath路径下的META-INF/services文件夹查找文件，自动加载文件里所定义的类。

更多介绍见：https://www.jianshu.com/p/3a3edbcd8f24

## 使用

定义一个接口：

```java
package cn.hutool.test.spi;

public interface SPIService {
    void execute();
}
```

有两个实现:

```java
package cn.hutool.test.spi;

public class SpiImpl1 implements SPIService{
    public void execute() {
        Console.log("SpiImpl1.execute()");
    }
}
```

```java
package cn.hutool.test.spi;

public class SpiImpl2 implements SPIService{
    public void execute() {
        Console.log("SpiImpl2.execute()");
    }
}
```

然后在classpath的`META-INF/services`下创建一个文件，叫`cn.hutool.test.spi.SPIService`，内容为：

```java
cn.hutool.test.spi.SpiImpl1
cn.hutool.test.spi.SpiImpl2
```

加载第一个可用服务，如果用户定义了多个接口实现类，只获取第一个不报错的服务。这个方法多用于同一接口多种实现的自动甄别加载，
通过判断jar是否引入，自动找到实现类。

```java
SPIService service = ServiceLoaderUtil.loadFirstAvailable(SPIService.class);
service.execute();
```
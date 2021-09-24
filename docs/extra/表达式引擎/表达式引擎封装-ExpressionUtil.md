## 介绍

与模板引擎类似，Hutool针对较为流行的表达式计算引擎封装为门面模式，提供统一的API，去除差异。
现有的引擎实现有：

- [Aviator](https://github.com/killme2008/aviatorscript)
- [Apache Jexl3](https://github.com/apache/commons-jexl)
- [MVEL](https://github.com/mvel/mvel)
- [JfireEL](https://gitee.com/eric_ds/jfireEL)
- [Rhino](https://github.com/mozilla/rhino)
- [Spring Expression Language (SpEL)](https://github.com/spring-projects/spring-framework/tree/master/spring-expression)

## 使用

首先引入我们需要的模板引擎，引入后，Hutool借助SPI机制可自动识别使用，我们以`Aviator`为例：

```java
<dependency>
	<groupId>com.googlecode.aviator</groupId>
	<artifactId>aviator</artifactId>
	<version>5.2.7</version>
</dependency>
```

### 执行表达式

```java
final Dict dict = Dict.create()
		.set("a", 100.3)
		.set("b", 45)
		.set("c", -199.100);

// -143.8
final Object eval = ExpressionUtil.eval("a-(b-c)", dict);
```

### 自定义引擎执行

如果项目中引入多个引擎，我们想选择某个引擎执行，则可以：

```java
ExpressionEngine engine = new JexlEngine();

final Dict dict = Dict.create()
		.set("a", 100.3)
		.set("b", 45)
		.set("c", -199.100);

// -143.8
final Object eval = engine.eval("a-(b-c)", dict);
```

### 创建自定义引擎

引擎的核心就是实现`ExpressionEngine`接口，此接口只有一个方法：`eval`。

我们实现此接口后，在项目的`META-INF/services/`下创建spi文件`cn.hutool.extra.expression.ExpressionEngine`：

```java
com.yourProject.XXXXEngine
```

这样就可以直接调用`ExpressionUtil.eval`执行表达式了。
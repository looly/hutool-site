## 介绍

CGLib (Code Generation Library) 是一个强大的,高性能,高质量的Code生成类库，通过此库可以完成动态代理、Bean拷贝等操作。

Hutool在`5.4.1`之后加入对Cglib的封装——`CglibUtil`，用于解决Bean拷贝的性能问题。

## 使用

### 引入Cglib

```xml
<dependency>
	<groupId>cglib</groupId>
	<artifactId>cglib</artifactId>
	<version>${cglib.version}</version>
	<scope>compile</scope>
</dependency>
```

### 使用

1. Bean拷贝

首先我们定义两个Bean：
```java
@Data
public class SampleBean {
	private String value;
}
@Data
public class OtherSampleBean {
	private String value;
}
```

> @Data是Lombok的注解，请自行补充get和set方法，或者引入Lombok依赖

```java
SampleBean bean = new SampleBean();
bean.setValue("Hello world");

OtherSampleBean otherBean = new OtherSampleBean();

CglibUtil.copy(bean, otherBean);

// 值为"Hello world"
otherBean.getValue();
```

当然，目标对象也可以省略，你可以传入一个class，让Hutool自动帮你实例化它：

```
OtherSampleBean otherBean2 = CglibUtil.copy(bean, OtherSampleBean.class);

// 值为"Hello world"
otherBean.getValue();
```

## 关于性能

Cglib的性能是目前公认最好的，其时间主要耗费在`BeanCopier`创建上，因此，Hutool根据传入Class不同，缓存了`BeanCopier`对象，使性能达到最好。
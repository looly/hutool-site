## 由来
使用Spring Boot时，通过依赖注入获取bean是非常方便的，但是在工具化的应用场景下，想要动态获取bean就变得非常困难，于是Hutool封装了Spring中Bean获取的工具类——SpringUtil。

## 使用

### 注册SpringUtil

1. 使用ComponentScan注册类

```java
// 扫描cn.hutool.extra.spring包下所有类并注册之
@ComponentScan(basePackages={"cn.hutool.extra.spring"})
```

2. 使用Import导入

```java
@Import(cn.hutool.extra.spring.SpringUtil.class)
```

### 获取指定Bean

1. 定义一个Bean

```java
@Data
	public static class Demo2{
		private long id;
		private String name;

		@Bean(name="testDemo")
		public Demo2 generateDemo() {
			Demo2 demo = new Demo2();
			demo.setId(12345);
			demo.setName("test");
			return demo;
		}
	}
```

2. 获取Bean
```java
final Demo2 testDemo = SpringUtil.getBean("testDemo");
```
## 介绍

在嵌套对象的属性获取中，由于子对象无法得知是否为`null`，每次获取属性都要检查属性兑现是否为null，使得代码会变得特备臃肿，因此使用`OptionalBean`来优雅的链式获取属性对象值。

> 声明：此类的实现来自：https://mp.weixin.qq.com/s/0c8iC0OTtx5LqPkhvkK0tw，PR来自：https://github.com/dromara/hutool/pull/1182

## 使用

我们先定义一个嵌套的Bean：

```java
// Lombok注解
@Data
public static class User {
	private String name;
	private String gender;
	private School school;
	@Data
	public static class School {
		private String name;
		private String address;
	}
}
```

假设我们想获取`address`属性，则：

```java
User user = new User();
user.setName("hello");

// null
String addressValue = OptionalBean.ofNullable(user)
		.getBean(User::getSchool)
		.getBean(User.School::getAddress).get();
```

由于school对象的值为`null`，一般直接获取会报空指针，使用`OptionalBean`即可避免判断。
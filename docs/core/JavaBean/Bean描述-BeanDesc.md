## 介绍

Hutool封装了Bean的信息描述来将一个Bean的相关信息全部通过反射解析出来，此类类似于JDK的`BeanInfo`，也可以理解为是这个类的强化版本。

BeanDesc包含所有字段（属性）及对应的Getter方法和Setter方法，与`BeanInfo`不同的是，`BeanDesc`要求属性和getter、setter必须严格对应，即如果有非public属性，没有getter，则不能获取属性值，没有setter也不能注入属性值。

属性和getter、setter关联规则如下：

1. 忽略字段和方法名的大小写（匹配时）
2. 字段名是XXX，则Getter查找getXXX、isXXX、getIsXXX
3. 字段名是XXX，Setter查找setXXX、setIsXXX
4. Setter忽略参数值与字段值不匹配的情况，因此有多个参数类型的重载时，会调用首次匹配的

## 使用

我们定义一个较为复杂的Bean：

```java
public static class User {
	private String name;
	private int age;
	private boolean isAdmin;
	private boolean isSuper;
	private boolean gender;

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public User setAge(int age) {
		this.age = age;
		return this;
	}
	public String testMethod() {
		return "test for " + this.name;
	}
	public boolean isAdmin() {
		return isAdmin;
	}
	public void setAdmin(boolean isAdmin) {
		this.isAdmin = isAdmin;
	}
	public boolean isIsSuper() {
		return isSuper;
	}
	public void setIsSuper(boolean isSuper) {
		this.isSuper = isSuper;
	}
	public boolean isGender() {
		return gender;
	}
	public void setGender(boolean gender) {
		this.gender = gender;
	}
	@Override
	public String toString() {
		return "User [name=" + name + ", age=" + age + ", isAdmin=" + isAdmin + ", gender=" + gender + "]";
	}
}
```

### 字段getter方法获取

1. 一般字段

```java
BeanDesc desc = BeanUtil.getBeanDesc(User.class);
// User
desc.getSimpleName();

// age
desc.getField("age").getName();
// getAge
desc.getGetter("age").getName();
// setAge
desc.getSetter("age").getName();
```

2. Boolean字段

我们会发现`User`中的boolean字段叫做`isAdmin`，此时同名的getter也可以获取到：

```java
BeanDesc desc = BeanUtil.getBeanDesc(User.class);

// isAdmin
desc.getGetter("isAdmin").getName()；
```

当然，用户如果觉得`isIsXXX`才是正确的，`BeanDesc`也可以完美获取，我们以`isSuper`字段为例：

```java
// isIsSuper
desc.getGetter("isSuper");
```

### 字段属性赋值

```java
BeanDesc desc = BeanUtil.getBeanDesc(User.class);
User user = new User();
desc.getProp("name").setValue(user, "张三");
```
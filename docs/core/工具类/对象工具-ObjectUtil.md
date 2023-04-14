## 由来

在我们的日常使用中，有些方法是针对Object通用的，这些方法不区分何种对象，针对这些方法，Hutool封装为`ObjectUtil`。

## 方法

### 默认值

借助于lambda表达式，ObjectUtil可以完成判断给定的值是否为null，不为null执行特定逻辑的功能。

```java
final String dateStr = null;

// 此处判断如果dateStr为null，则调用`Instant.now()`，不为null则执行`DateUtil.parse`
Instant result1 = ObjectUtil.defaultIfNull(dateStr,
		() -> DateUtil.parse(dateStr, DatePattern.NORM_DATETIME_PATTERN).toInstant(), Instant.now());
```

### `ObjectUtil.equal`
比较两个对象是否相等，相等需满足以下条件之一：

1. obj1 == null && obj2 == null
2. obj1.equals(obj2)

```java
Object a = null;
Object b = null;

// true
ObjectUtil.equals(a, b);
```

### `ObjectUtil.length`
计算对象长度，如果是字符串调用其length方法，集合类调用其size方法，数组调用其length属性，其他可遍历对象遍历计算长度。

支持的类型包括：
- CharSequence
- Collection
- Map
- Iterator
- Enumeration
- Array

```java
int[] array = new int[]{1,2,3,4,5};

// 5
int length = ObjectUtil.length(array);

Map<String, String> map = new HashMap<>();
map.put("a", "a1");
map.put("b", "b1");
map.put("c", "c1");

// 3
length = ObjectUtil.length(map);
```

### `ObjectUtil.contains`
对象中是否包含元素。

支持的对象类型包括：
- String
- Collection
- Map
- Iterator
- Enumeration
- Array

```java
int[] array = new int[]{1,2,3,4,5};

// true
final boolean contains = ObjectUtil.contains(array, 1);
```

### 判断是否为null
- `ObjectUtil.isNull`
- `ObjectUtil.isNotNull`

> 注意：此方法不能判断对象中字段为空的情况，如果需要检查Bean对象中字段是否全空，请使用`BeanUtil.isEmpty`。

### 克隆

- `ObjectUtil.clone` 克隆对象，如果对象实现Cloneable接口，调用其clone方法，如果实现Serializable接口，执行深度克隆，否则返回`null`。

```java
class Obj extends CloneSupport<Obj> {
	public String doSomeThing() {
		return "OK";
	}
}
```

```java
Obj obj = new Obj();
Obj obj2 = ObjectUtil.clone(obj);

// OK
obj2.doSomeThing();
```

- `ObjectUtil.cloneIfPossible` 返回克隆后的对象，如果克隆失败，返回原对象

- `ObjectUtil.cloneByStream` 序列化后拷贝流的方式克隆，对象必须实现Serializable接口

### 序列化和反序列化

- `serialize` 序列化，调用JDK序列化
- `deserialize`  反序列化，调用JDK

### 判断基本类型

`ObjectUtil.isBasicType` 判断是否为基本类型，包括包装类型和原始类型。

包装类型：

- Boolean
- Byte
- Character
- Double
- Float
- Integer
- Long
- Short

原始类型：

- boolean
- byte
- char
- double
- float
- int
- long
- short

```java
int a = 1;

// true
final boolean basicType = ObjectUtil.isBasicType(a);
```
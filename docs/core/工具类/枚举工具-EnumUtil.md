## 介绍
枚举（enum）算一种“语法糖”，是指一个经过排序的、被打包成一个单一实体的项列表。一个枚举的实例可以使用枚举项列表中任意单一项的值。枚举在各个语言当中都有着广泛的应用，通常用来表示诸如颜色、方式、类别、状态等等数目有限、形式离散、表达又极为明确的量。Java从JDK5开始，引入了对枚举的支持。

`EnumUtil` 用于对未知枚举类型进行操作。

## 方法

首先我们定义一个枚举对象：

```java
//定义枚举
public enum TestEnum{
	TEST1("type1"), TEST2("type2"), TEST3("type3");
	
	private TestEnum(String type) {
		this.type = type;
	}
	
	private String type;
	
	public String getType() {
		return this.type;
	}
}
```

### `getNames`

获取枚举类中所有枚举对象的name列表。栗子：

```java
List<String> names = EnumUtil.getNames(TestEnum.class);
//结果：[TEST1, TEST2, TEST3]
```

### `getFieldValues`

获得枚举类中各枚举对象下指定字段的值。栗子：

```java
List<Object> types = EnumUtil.getFieldValues(TestEnum.class, "type");
//结果：[type1, type2, type3]
```

### `getBy`
根据传入lambda和值获得对应枚举。栗子🌰：
```java
TestEnum testEnum = EnumUtil.getBy(TestEnum::ordinal, 1);
//结果：TEST2
```

### `getFieldBy`
根据传入lambda和值获得对应枚举的值。栗子🌰：
```java
String type = EnumUtil.getFieldBy(TestEnum::getType, Enum::ordinal, 1);
// 结果：“type2”
```

### `getEnumMap`

获取枚举字符串值和枚举对象的Map对应，使用LinkedHashMap保证有序，结果中键为枚举名，值为枚举对象。栗子：

```java
Map<String,TestEnum> enumMap = EnumUtil.getEnumMap(TestEnum.class);
enumMap.get("TEST1") // 结果为：TestEnum.TEST1
```

### `getNameFieldMap`

获得枚举名对应指定字段值的Map，键为枚举名，值为字段值。栗子：

```java
Map<String, Object> enumMap = EnumUtil.getNameFieldMap(TestEnum.class, "type");
enumMap.get("TEST1") // 结果为：type1
```
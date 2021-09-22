## 介绍

在JSON中，JSONArray代表一个数组，使用中括号包围，每个元素使用逗号隔开。一个JSONArray类似于这样：
```json
["value1","value2","value3"]
```

## 使用

### 创建

```java
//方法1
JSONArray array = JSONUtil.createArray();
//方法2
JSONArray array = new JSONArray();

array.add("value1");
array.add("value2");
array.add("value3");

//转为JSONArray字符串
array.toString();
```

### 从Bean列表解析

先定义bean：

```java
@Data
public class KeyBean{
	private String akey;
	private String bkey;
}
```

```java
KeyBean b1 = new KeyBean();
b1.setAkey("aValue1");
b1.setBkey("bValue1");
KeyBean b2 = new KeyBean();
b2.setAkey("aValue2");
b2.setBkey("bValue2");

ArrayList<KeyBean> list = CollUtil.newArrayList(b1, b2);

// [{"akey":"aValue1","bkey":"bValue1"},{"akey":"aValue2","bkey":"bValue2"}]
JSONArray jsonArray = JSONUtil.parseArray(list);

// aValue1
jsonArray.getJSONObject(0).getStr("akey");
```

### 从JSON字符串解析

```java
String jsonStr = "[\"value1\", \"value2\", \"value3\"]";
JSONArray array = JSONUtil.parseArray(jsonStr);
```

### 转换为bean的List

先定义一个Bean

```java
@Data
static class User {
	private Integer id;
	private String name;
}
```

```java
String jsonArr = "[{\"id\":111,\"name\":\"test1\"},{\"id\":112,\"name\":\"test2\"}]";
JSONArray array = JSONUtil.parseArray(jsonArr);

List<User> userList = JSONUtil.toList(array, User.class);

// 111
userList.get(0).getId();
```

### 转换为Dict的List

Dict是Hutool定义的特殊Map，提供了以字符串为key的Map功能，并提供getXXX方法，转换也类似：

```java
String jsonArr = "[{\"id\":111,\"name\":\"test1\"},{\"id\":112,\"name\":\"test2\"}]";
JSONArray array = JSONUtil.parseArray(jsonArr);

List<Dict> list = JSONUtil.toList(array, Dict.class);

// 111
list.get(0).getInt("id");
```

### 转换为数组

```java
String jsonArr = "[{\"id\":111,\"name\":\"test1\"},{\"id\":112,\"name\":\"test2\"}]";
JSONArray array = JSONUtil.parseArray(jsonArr);

User[] list = array.toArray(new User[0]);
```

### JSON路径

如果JSON的层级特别深，那么获取某个值就变得非常麻烦，代码也很臃肿，Hutool提供了`getByPath`方法可以通过表达式获取JSON中的值。

```java
String jsonStr = "[{\"id\": \"1\",\"name\": \"a\"},{\"id\": \"2\",\"name\": \"b\"}]";
final JSONArray jsonArray = JSONUtil.parseArray(jsonStr);

// b
jsonArray.getByPath("[1].name");
```
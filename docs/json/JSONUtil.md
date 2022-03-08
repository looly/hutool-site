JSONUtil
===

## 介绍

`JSONUtil`是针对JSONObject和JSONArray的静态快捷方法集合，在之前的章节我们已经介绍了一些工具方法，在本章节我们将做一些补充。

## 使用

### JSON字符串创建

`JSONUtil.toJsonStr`可以将任意对象（Bean、Map、集合等）直接转换为JSON字符串。
如果对象是有序的Map等对象，则转换后的JSON字符串也是有序的。

```java
SortedMap<Object, Object> sortedMap = new TreeMap<Object, Object>() {
	private static final long serialVersionUID = 1L;
	{
	put("attributes", "a");
	put("b", "b");
	put("c", "c");
}};

JSONUtil.toJsonStr(sortedMap);
```

结果：

```json
{"attributes":"a","b":"b","c":"c"}
```

如果我们想获得格式化后的JSON，则：

```java
JSONUtil.toJsonPrettyStr(sortedMap);
```

结果：

```json
{
    "attributes": "a",
    "b": "b",
    "c": "c"
}
```

### JSON字符串解析

```java
String html = "{\"name\":\"Something must have been changed since you leave\"}";
JSONObject jsonObject = JSONUtil.parseObj(html);
jsonObject.getStr("name");
```

### XML字符串转换为JSON

```java
String s = "<sfzh>123</sfzh><sfz>456</sfz><name>aa</name><gender>1</gender>";
JSONObject json = JSONUtil.parseFromXml(s);

json.get("sfzh");
json.get("name");
```

### JSON转换为XML

```java
final JSONObject put = JSONUtil.createObj()
		.set("aaa", "你好")
		.set("键2", "test");

// <aaa>你好</aaa><键2>test</键2>
final String s = JSONUtil.toXmlStr(put);
```

### JSON转Bean

我们先定义两个较为复杂的Bean（包含泛型）

```java
@Data
public class ADT {
	private List<String> BookingCode;
}

@Data
public class Price {
	private List<List<ADT>> ADT;
}
```

```java
String json = "{\"ADT\":[[{\"BookingCode\":[\"N\",\"N\"]}]]}";

Price price = JSONUtil.toBean(json, Price.class);

// 
price.getADT().get(0).get(0).getBookingCode().get(0);
```

### Bean转JSON

5.x的Hutool中增加了一个自定义注解：`@Alias`，通过此注解可以给Bean的字段设置别名。

```java
@Data
public class Test {
    private String name;

    @Alias("aliasSex")
    private String sex;

    public static void main(String[] args) {
        Test test = new Test();
        test.setName("handy");
        test.setSex("男");
        // 结果: {"name":"handy","aliasSex":"男"}
        String json = JSONUtil.toJsonStr(test);
    }

}

```

### readXXX

这类方法主要是从JSON文件中读取JSON对象的快捷方法。包括：
- readJSON
- readJSONObject
- readJSONArray

### 其它方法

除了上面中常用的一些方法，JSONUtil还提供了一些JSON辅助方法：
- quote 对所有双引号做转义处理（使用双反斜杠做转义）
- wrap 包装对象，可以将普通任意对象转为JSON对象
- formatJsonStr 格式化JSON字符串，此方法并不严格检查JSON的格式正确与否


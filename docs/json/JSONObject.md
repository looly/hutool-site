## 介绍
JSONObject代表一个JSON中的键值对象，这个对象以大括号包围，每个键值对使用`,`隔开，键与值使用`:`隔开，一个JSONObject类似于这样：
```json
{
  "key1":"value1",
  "key2":"value2"
}
```

此处键部分可以省略双引号，值为字符串时不能省略，为数字或波尔值时不加双引号。

## 使用

### 创建
```java
JSONObject json1 = JSONUtil.createObj()
  .put("a", "value1")
  .put("b", "value2")
  .put("c", "value3");
```

`JSONUtil.createObj()`是快捷新建JSONObject的工具方法，同样我们可以直接new：

```java
JSONObject json1 = new JSONObject();
...
```

### 转换

1. JSON字符串解析
```java
String jsonStr = "{\"b\":\"value2\",\"c\":\"value3\",\"a\":\"value1\"}";
//方法一：使用工具类转换
JSONObject jsonObject = JSONUtil.parseObj(jsonStr);
//方法二：new的方式转换
JSONObject jsonObject2 = new JSONObject(jsonStr);

//JSON对象转字符串（一行）
jsonObject.toString();

// 也可以美化一下，即显示出带缩进的JSON：
jsonObject.toStringPretty();
```

2. JavaBean解析

首先我们定义一个Bean
```java
// 注解使用Lombok
@Data
public class UserA {
	private String name;
	private String a;
	private Date date;
	private List<Seq> sqs;
}
```

解析为JSON：
```java
UserA userA = new UserA();
userA.setName("nameTest");
userA.setDate(new Date());
userA.setSqs(CollectionUtil.newArrayList(new Seq(null), new Seq("seq2")));

// false表示不跳过空值
JSONObject json = JSONUtil.parseObj(userA, false);
Console.log(json.toStringPretty());
```

结果：

```json
{
    "date": 1585618492295,
    "a": null,
    "sqs": [
        {
            "seq": null
        },
        {
            "seq": "seq2"
        }
    ],
    "name": "nameTest"
}
```

可以看到，输出的字段顺序和Bean的字段顺序不一致，如果想保持一致，可以：

```java
// 第二个参数表示保持有序
JSONObject json = JSONUtil.parseObj(userA, false, true);
```

结果：

```json
{
    "name": "nameTest",
    "a": null,
    "date": 1585618648523,
    "sqs": [
        {
            "seq": null
        },
        {
            "seq": "seq2"
        }
    ]
}
```

默认的，Hutool将日期输出为时间戳，如果需要自定义日期格式，可以调用：

```java
json.setDateFormat("yyyy-MM-dd HH:mm:ss");
```

得到结果为：

```json
{
    "name": "nameTest",
    "a": null,
    "date": "2020-03-31 09:41:29",
    "sqs": [
        {
            "seq": null
        },
        {
            "seq": "seq2"
        }
    ]
}
```

同样，`JSONUtil`还可以支持以下对象转为JSONObject对象：
- String对象
- Java Bean对象
- Map对象
- XML字符串（使用`JSONUtil.parseFromXml`方法）
- ResourceBundle(使用`JSONUtil.parseFromResourceBundle`)

`JSONUtil`还提供了JSONObject对象转换为其它对象的方法：
- toJsonStr 转换为JSON字符串
- toXmlStr 转换为XML字符串
- toBean 转换为JavaBean
- 


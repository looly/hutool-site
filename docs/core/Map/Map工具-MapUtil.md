## 介绍
MapUtil是针对Map的一一列工具方法的封装，包括getXXX的快捷值转换方法。

## 方法

- `isEmpty`、`isNotEmpty` 判断Map为空和非空方法，空的定义为null或没有值
- `newHashMap` 快速创建多种类型的HashMap实例
- `createMap` 创建自定义的Map类型的Map
- `of` 此方法将一个或多个键值对加入到一个新建的Map中，下面是栗子:

```java
Map<Object, Object> colorMap = MapUtil.of(new String[][] {
     {"RED", "#FF0000"},
     {"GREEN", "#00FF00"},
     {"BLUE", "#0000FF"}
});
```

- `toListMap` 行转列，合并相同的键，值合并为列表，将Map列表中相同key的值组成列表做为Map的value，例如传入数据是：

```json
[
  {a: 1, b: 1, c: 1},
  {a: 2, b: 2},
  {a: 3, b: 3},
  {a: 4}
]
```

结果为：

```json
{
   a: [1,2,3,4],
   b: [1,2,3,],
   c: [1]
}
```

- `toMapList` 列转行。将Map中值列表分别按照其位置与key组成新的map，例如传入数据：

```json
{
   a: [1,2,3,4],
   b: [1,2,3,],
   c: [1]
}
```

结果为：
```json
[
  {a: 1, b: 1, c: 1},
  {a: 2, b: 2},
  {a: 3, b: 3},
  {a: 4}
]
```

- `join`、`joinIgnoreNull`、`sortJoin`将Map按照给定的分隔符转换为字符串，此方法一般用于签名。

```java
Map<String, String> build = MapUtil.builder(new HashMap<String, String>())
	.put("key1", "value1")
	.put("key3", "value3")
	.put("key2", "value2").build();

// key1value1key2value2key3value3
String join1 = MapUtil.sortJoin(build, StrUtil.EMPTY, StrUtil.EMPTY, false);
// key1value1key2value2key3value3123
String join2 = MapUtil.sortJoin(build, StrUtil.EMPTY, StrUtil.EMPTY, false, "123");
```

- `filter` 过滤过程通过传入的Editor实现来返回需要的元素内容，这个Editor实现可以实现以下功能：1、过滤出需要的对象，如果返回null表示这个元素对象抛弃 2、修改元素对象，返回集合中为修改后的对象

```java
Map<String, String> map = MapUtil.newHashMap();
map.put("a", "1");
map.put("b", "2");
map.put("c", "3");
map.put("d", "4");

Map<String, String> map2 = MapUtil.filter(map, (Filter<Entry<String, String>>) t -> Convert.toIn(t.getValue()) % 2 == 0);
```

结果为

```json
{
   b: "2",
   d: "4"
}
```

- `reverse` Map的键和值互换

```java
Map<String, String> map = MapUtil.newHashMap();
		map.put("a", "1");
		map.put("b", "2");
		map.put("c", "3");
		map.put("d", "4");

Map<String, String> map2 = MapUtil.reverse(map);
```

结果为：
```json
{
   "1": "a",
   "2": "b",
   "3": "c",
   "4": "d",
}
```

- `sort` 排序Map
- `getAny` 获取Map的部分key生成新的Map
- `get`、`getXXX` 获取Map中指定类型的值

## 介绍

有时候我们需要键值对一一对应，但是又有可能有重复的键，也可能有重复的值，就像一个2列的表格一样：

|  键   |   值   |
| ----- | ------ |
| key1  | value1 |
| key2  | value2 |

因此，Hutool创建了`TableMap`这类数据结构，通过键值单独建立List方式，使键值对一一对应，实现正向和反向两种查找。

当然，这种Map无论是正向还是反向，都是遍历列表查找过程，相比标准的HashMap要慢，数据越多越慢。

## 使用

```java
TableMap<String, Integer> tableMap = new TableMap<>(new HashMap<>());
tableMap.put("aaa", 111);
tableMap.put("bbb", 222);

// 111
tableMap.get("aaa");
// 222
tableMap.get("bbb");

// aaa
tableMap.getKey(111);
// bbb
tableMap.getKey(222);

// [111]
tableMap.getValues("aaa");

//[aaa]
tableMap.getKeys(111);
```
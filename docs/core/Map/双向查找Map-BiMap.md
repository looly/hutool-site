## 介绍

我们知道在Guava中提供了一种特殊的Map结构，叫做BiMap，它实现了一种双向查找的功能，即根据key查找value和根据value查找key，Hutool也同样提供此对象。

BiMap要求key和value都不能重复（非强制要求），如果key重复了，后加入的键值对会覆盖之前的键值对，如果value重复了，则会按照不确定的顺序覆盖key，这完全取决于map实现。比如HashMap无序（按照hash顺序），则谁覆盖谁和hash算法有关；如果是LinkedHashMap，则有序，是后加入的覆盖先加入的。

## 使用

```java
BiMap<String, Integer> biMap = new BiMap<>(new HashMap<>());
biMap.put("aaa", 111);
biMap.put("bbb", 222);

// 111
biMap.get("aaa");
// 222
biMap.get("bbb");

// aaa
biMap.getKey(111);
// bbb
biMap.getKey(222);
```
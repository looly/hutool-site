## 介绍

List在集合中中使用最为频繁，因此新版本的Hutool中针对`List`单独封装了工具方法。

## 使用

### 过滤列表

```java
List<String> a = ListUtil.toLinkedList("1", "2", "3");
// 结果: [edit1, edit2, edit3]
List<String> filter = ListUtil.filter(a, str -> "edit" + str);
```

### 获取满足指定规则所有的元素的位置

```java
List<String> a = ListUtil.toLinkedList("1", "2", "3", "4", "3", "2", "1");
// [1, 5]
int[] indexArray = ListUtil.indexOfAll(a, "2"::equals);
```

其他方法与`CollUtil`工具类似，很多工具也有重复。
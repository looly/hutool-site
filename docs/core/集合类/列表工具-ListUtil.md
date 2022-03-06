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

### 拆分

对集合按照指定长度分段，每一个段为单独的集合，返回这个集合的列表：

```java
List<List<Object>> lists = ListUtil.split(Arrays.asList(1, 2, 3, 4), 1);
List<List<Object>> lists = ListUtil.split(null, 3);
```

也可以平均拆分，即平均分成N份，每份的数量差不超过1：

```java
// [[1, 2, 3, 4]]
List<List<Object>> lists = ListUtil.splitAvg(Arrays.asList(1, 2, 3, 4), 1);

// [[1, 2], [3], [4]]
lists = ListUtil.splitAvg(Arrays.asList(1, 2, 3, 4), 3);
```

### 编辑元素

我们可以针对集合中所有元素按照给定的lambda定义规则修改元素：

```java
List<String> a = ListUtil.toLinkedList("1", "2", "3");
final List<String> filter = (List<String>) CollUtil.edit(a, str -> "edit" + str);

// edit1
filter.get(0);
```

### 查找位置

```java
List<String> a = ListUtil.toLinkedList("1", "2", "3", "4", "3", "2", "1");

// 查找所有2的位置
// [1,5]
final int[] indexArray = ListUtil.indexOfAll(a, "2"::equals);
```

### 列表截取

```java
final List<Integer> of = ListUtil.of(1, 2, 3, 4);

// [3, 4]
final List<Integer> sub = ListUtil.sub(of, 2, 4);

// 对子列表操作不影响原列表
sub.remove(0);
```

### 排序

如我们想按照bean对象的order字段值排序：

```java
@Data
@AllArgsConstructor
class TestBean{
	private int order;
	private String name;
}

final List<TestBean> beanList = ListUtil.toList(
		new TestBean(2, "test2"),
		new TestBean(1, "test1"),
		new TestBean(5, "test5"),
		new TestBean(4, "test4"),
		new TestBean(3, "test3")
		);

final List<TestBean> order = ListUtil.sortByProperty(beanList, "order");
```

### 元素交换

```java
List<Integer> list = Arrays.asList(7, 2, 8, 9);

// 将元素8和第一个位置交换
ListUtil.swapTo(list, 8, 1);
```

### 分页
```java
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
list.add("c");
List<String> page = ListUtil.page(1, 2, list);
```

### 分组
```java
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
list.add("c");
List<List<String>> partition = ListUtil.partition(list, 2);
```
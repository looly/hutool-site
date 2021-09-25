## 介绍

Java8中的新特性之一就是Stream，Hutool针对常用操作做了一些封装

## 使用

### 集合转Map

```java
@Data
@AllArgsConstructor
@ToString
public static class Student {
	private long termId;//学期id
	private long classId;//班级id
	private long studentId;//班级id
	private String name;//学生名称
}
```

我们可以建立一个学生id和学生对象之间的map：

```java
List<Student> list = new ArrayList<>();
list.add(new Student(1, 1, 1, "张三"));
list.add(new Student(1, 1, 2, "李四"));
list.add(new Student(1, 1, 3, "王五"));

Map<Long, Student> map = CollStreamUtil.toIdentityMap(list, Student::getStudentId);

// 张三
map.get(1L).getName();
```

我们也可以自定义Map的key和value放的内容，如我们可以将学生信息的id和姓名生成map：

```java
Map<Long, String> map = map = CollStreamUtil.toMap(list, Student::getStudentId, Student::getName);

// 张三
map.get(1L);
```

### 分组

我们将学生按照班级分组：

```java
List<Student> list = new ArrayList<>();
list.add(new Student(1, 1, 1, "张三"));
list.add(new Student(1, 2, 2, "李四"));
list.add(new Student(2, 1, 1, "擎天柱"));
list.add(new Student(2, 2, 2, "威震天"));
list.add(new Student(2, 3, 2, "霸天虎"));

Map<Long, List<Student>> map = CollStreamUtil.groupByKey(list, Student::getClassId);
```

### 转换提取

我们可以将学生信息列表转换提取为姓名的列表：

```java
List<String> list = CollStreamUtil.toList(null, Student::getName);
```

### 合并

合并两个相同key类型的map，可自定义合并的lambda，将key  value1 value2合并成最终的类型,注意value可能为空的情况。

```java
Map<Long, Student> map1 = new HashMap<>();
map1.put(1L, new Student(1, 1, 1, "张三"));

Map<Long, Student> map2 = new HashMap<>();
map2.put(1L, new Student(2, 1, 1, "李四"));
```

定义merge规则：

```java
private String merge(Student student1, Student student2) {
	if (student1 == null && student2 == null) {
		return null;
	} else if (student1 == null) {
		return student2.getName();
	} else if (student2 == null) {
		return student1.getName();
	} else {
		return student1.getName() + student2.getName();
	}

```

```java
Map<Long, String> map = CollStreamUtil.merge(map1, map2, this::merge);
```
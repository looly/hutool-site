## 介绍

MapBuilder提供了一种流式的Map创建方法。

## 使用

```java
Map<String, Object> srcMap = MapBuilder
	.create(new HashMap<String, Object>())
	.put("name", "AAA")
	.put("age", 45).map();
```
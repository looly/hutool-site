## 由来

文件名操作工具类，主要针对文件名获取主文件名、扩展名等操作，同时针对Windows平台，清理无效字符。

此工具类在`5.4.1`之前是`FileUtil`的一部分，后单独剥离为`FileNameUtil`工具。

## 使用

1. 获取文件名

```java
File file = FileUtil.file("/opt/test.txt");

// test.txt
String name = FileNameUtil.getName(file);
```

2. 获取主文件名和扩展名

```java
File file = FileUtil.file("/opt/test.txt");

// "test"
String name = FileNameUtil.mainName(file);

// "txt"
String name = FileNameUtil.extName(file);
```

> 注意，此处获取的扩展名不带`.`。
> `FileNameUtil.mainName`和`FileNameUtil.getPrefix`等价，同理`FileNameUtil.extName`和`FileNameUtil.getSuffix`等价，保留两个方法用于适应不同用户的习惯。
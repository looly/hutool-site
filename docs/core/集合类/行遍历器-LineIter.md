## 介绍

此工具分别参考和`Apache Commons io`和`Guava`项目。

将Reader包装为一个按照行读取的Iterator。

### 使用

```java
final LineIter lineIter = new LineIter(ResourceUtil.getUtf8Reader("test_lines.csv"));

for (String line : lineIter) {
	Console.log(line);
}
```
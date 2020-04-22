## 简介

我们知道，JDK提供了线程安全的HashMap：ConcurrentHashMap，但是没有提供对应的ConcurrentHashSet，Hutool借助ConcurrentHashMap封装了线程安全的ConcurrentHashSet。

## 使用

与普通的HashSet使用一致:

```java
Set<String> set = new ConcurrentHashSet<>();
set.add("a");
set.add("b");
```
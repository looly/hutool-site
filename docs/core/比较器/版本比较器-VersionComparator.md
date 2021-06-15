## 介绍

版本比较器用于比较版本号，支持的格式包括：

- x.x.x(1.3.20)
- x.x.yyyyMMdd(6.82.20160101)
- 带字母的版本(8.5a/8.5c)
- 带V的版本(V8.5)

## 使用

```java
// -1
int compare = VersionComparator.INSTANCE.compare("1.12.1", "1.12.1c");
```

```java
// 1
int compare = VersionComparator.INSTANCE.compare("V0.0.20170102", "V0.0.20170101");
```
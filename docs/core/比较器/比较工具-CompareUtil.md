## 介绍

在JDK提供的比较器中，对于`null`的比较没有考虑，Hutool封装了相关比较，可选null是按照最大值还是最小值对待。

```java
// 当isNullGreater为true时，null始终最大，此处返回的compare > 0
int compare = CompareUtil.compare(null, "a", true);
```

```java
// 当isNullGreater为false时，null始终最小，此处返回的compare < 0
int compare = CompareUtil.compare(null, "a", false);
```
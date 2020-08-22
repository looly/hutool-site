## 介绍

从Hutool的5.4.x开始，Hutool加入了针对JDK8+日期API的封装，此工具类的功能包括`LocalDateTime`和`LocalDate`的解析、格式化、转换等操作。

## 使用

1. 日期转换

```java
String dateStr = "2020-01-23T12:23:56";
DateTime dt = DateUtil.parse(dateStr);

// Date对象转换为LocalDateTime
LocalDateTime of = LocalDateTimeUtil.of(dt);

// 时间戳转换为LocalDateTime
of = LocalDateTimeUtil.ofUTC(dt.getTime());
```

2. 日期字符串解析

```java
// 解析ISO时间
LocalDateTime localDateTime = LocalDateTimeUtil.parse("2020-01-23T12:23:56");


// 解析自定义格式时间
localDateTime = LocalDateTimeUtil.parse("2020-01-23", DatePattern.NORM_DATE_PATTERN);
```

解析同样支持`LocalDate`：

```java
LocalDate localDate = LocalDateTimeUtil.parseDate("2020-01-23");

// 解析日期时间为LocalDate，时间部分舍弃
localDate = LocalDateTimeUtil.parseDate("2020-01-23T12:23:56", DateTimeFormatter.ISO_DATE_TIME);
```

3. 日期格式化

```java
LocalDateTime localDateTime = LocalDateTimeUtil.parse("2020-01-23T12:23:56");

// "2020-01-23 12:23:56"
String format = LocalDateTimeUtil.format(localDateTime, DatePattern.NORM_DATETIME_PATTERN);
```

4. 日期偏移

```java
final LocalDateTime localDateTime = LocalDateTimeUtil.parse("2020-01-23T12:23:56");

// 增加一天
// "2020-01-24T12:23:56"
LocalDateTime offset = LocalDateTimeUtil.offset(localDateTime, 1, ChronoUnit.DAYS);
```

如果是减少时间，offset第二个参数传负数即可：

```java
// "2020-01-22T12:23:56"
offset = LocalDateTimeUtil.offset(localDateTime, -1, ChronoUnit.DAYS);
```

5. 计算时间间隔

```java
LocalDateTime start = LocalDateTimeUtil.parse("2019-02-02T00:00:00");
LocalDateTime end = LocalDateTimeUtil.parse("2020-02-02T00:00:00");

Duration between = LocalDateTimeUtil.between(start, end);

// 365
between.toDays();
```

6. 一天的开始和结束

```java
LocalDateTime localDateTime = LocalDateTimeUtil.parse("2020-01-23T12:23:56");

// "2020-01-23T00:00"
LocalDateTime beginOfDay = LocalDateTimeUtil.beginOfDay(localDateTime);

// "2020-01-23T23:59:59.999999999"
LocalDateTime endOfDay = LocalDateTimeUtil.endOfDay(localDateTime);
```
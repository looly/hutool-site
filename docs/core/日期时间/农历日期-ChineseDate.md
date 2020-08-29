## 介绍

农历日期，提供了生肖、天干地支、传统节日等方法。

## 使用

1. 构建`ChineseDate`对象

`ChineseDate`表示了农历的对象，构建此对象既可以使用公历的日期，也可以使用农历的日期。

```java
//通过农历构建
ChineseDate chineseDate = new ChineseDate(1992,12,14);

//通过公历构建
ChineseDate chineseDate = new ChineseDate(DateUtil.parseDate("1993-01-06"));
```

2. 基本使用

```java
//通过公历构建
ChineseDate date = new ChineseDate(DateUtil.parseDate("2020-01-25"));
// 一月
date.getChineseMonth();
// 正月
date.getChineseMonthName();
// 初一
date.getChineseDay();
// 庚子
date.getCyclical();
// 生肖：鼠
date.getChineseZodiac();
// 传统节日（部分支持，逗号分隔）：春节
date.getFestivals();
// 庚子鼠年 正月初一
date.toString();
```

3. 获取天干地支

从`5.4.1`开始，Hutool支持天干地支的获取：

```java
//通过公历构建
ChineseDate chineseDate = new ChineseDate(DateUtil.parseDate("2020-08-28"));

// 庚子年甲申月癸卯日
String cyclicalYMD = chineseDate.getCyclicalYMD();
```
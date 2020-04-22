## 介绍

农历日期，提供了生肖、天干地支、传统节日等方法。

## 使用

```java
ChineseDate date = new ChineseDate(DateUtil.parseDate("2020-01-25"));
// 一月
date.getChineseMonth();
// 正月
date.getChineseMonthName();
// 初一
date.getChineseDay();
// 庚子
date.getCyclical();
// 鼠
date.getChineseZodiac();
// 春节
date.getFestivals();
// 庚子鼠年 正月初一
date.toString();
```
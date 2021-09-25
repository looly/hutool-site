## 介绍

BCD码（Binary-Coded Decimal‎）亦称二进码十进数或二-十进制代码。

BCD码这种编码形式利用了四个位元来储存一个十进制的数码，使二进制和十进制之间的转换得以快捷的进行。

## 使用

```java
String strForTest = "123456ABCDEF";

// 转BCD
byte[] bcd = BCD.strToBcd(strForTest);

// 解码BCD
String str = BCD.bcdToStr(bcd);
```
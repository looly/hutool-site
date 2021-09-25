## 介绍

Punycode是一个根据RFC 3492标准而制定的编码系统，主要用于把域名从地方语言所采用的Unicode编码转换成为可用于DNS系统的编码。

具体见：[RFC 3492](https://www.ietf.org/rfc/rfc3492.html)

## 使用

```java
String text = "Hutool编码器";

// Hutool-ux9js33tgln
String strPunyCode = PunyCode.encode(text);

// Hutool编码器
String decode = PunyCode.decode("Hutool-ux9js33tgln");

// Hutool编码器
decode = PunyCode.decode("xn--Hutool-ux9js33tgln");
```
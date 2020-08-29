## 介绍

### HMAC介绍
HMAC，全称为“Hash Message Authentication Code”，中文名“散列消息鉴别码”，主要是利用哈希算法，以一个密钥和一个消息为输入，生成一个消息摘要作为输出。一般的，消息鉴别码用于验证传输于两个共 同享有一个密钥的单位之间的消息。HMAC 可以与任何迭代散列函数捆绑使用。MD5 和 SHA-1 就是这种散列函数。HMAC 还可以使用一个用于计算和确认消息鉴别值的密钥。

## Hutool支持的算法类型

### Hmac算法

在不引入第三方库的情况下，JDK支持有限的摘要算法：

- HmacMD5
- HmacSHA1
- HmacSHA256
- HmacSHA384
- HmacSHA512

## 使用

### HMac

以HmacMD5为例：
```java
String testStr = "test中文";

// 此处密钥如果有非ASCII字符，考虑编码
byte[] key = "password".getBytes();
HMac mac = new HMac(HmacAlgorithm.HmacMD5, key);

// b977f4b13f93f549e06140971bded384
String macHex1 = mac.digestHex(testStr);
```

## 更多HMac算法

与摘要算法类似，通过加入`Bouncy Castle`库可以调用更多算法，使用也类似：

```java
HMac mac = new HMac("XXXX", key);
```
## 介绍

### 摘要算法介绍

摘要算法是一种能产生特殊输出格式的算法，这种算法的特点是：无论用户输入什么长度的原始数据，经过计算后输出的密文都是固定长度的，这种算法的原理是根据一定的运算规则对原数据进行某种形式的提取，这种提取就是摘要，被摘要的数据内容与原数据有密切联系，只要原数据稍有改变，输出的“摘要”便完全不同，因此，基于这种原理的算法便能对数据完整性提供较为健全的保障。

但是，由于输出的密文是提取原数据经过处理的定长值，所以它已经不能还原为原数据，即消息摘要算法是不可逆的，理论上无法通过反向运算取得原数据内容，因此它通常只能被用来做数据完整性验证。

## Hutool支持的摘要算法类型

在不引入第三方库的情况下，JDK支持有限的摘要算法：

详细见：[https://docs.oracle.com/javase/7/docs/technotes/guides/security/StandardNames.html#MessageDigest](https://docs.oracle.com/javase/7/docs/technotes/guides/security/StandardNames.html#MessageDigest)

### 摘要算法
- MD2
- MD5
- SHA-1
- SHA-256
- SHA-384
- SHA-512

## 使用

### Digester

以MD5为例：
```java
Digester md5 = new Digester(DigestAlgorithm.MD5);

// 5393554e94bf0eb6436f240a4fd71282
String digestHex = md5.digestHex(testStr);
```

当然，做为最为常用的方法，MD5等方法被封装为工具方法在`DigestUtil`中，以上代码可以进一步简化为：

```java
// 5393554e94bf0eb6436f240a4fd71282
String md5Hex1 = DigestUtil.md5Hex(testStr);
```

## 更多摘要算法

### SM3

在`4.2.1`之后，Hutool借助`Bouncy Castle`库可以支持国密算法，以SM3为例：

我们首先需要引入Bouncy Castle库：

```xml
<dependency>
  <groupId>org.bouncycastle</groupId>
  <artifactId>bcprov-jdk15to18</artifactId>
  <version>1.66</version>
</dependency>
```

然后可以调用SM3算法，调用方法与其它摘要算法一致：

```java
Digester digester = DigestUtil.digester("sm3");

//136ce3c86e4ed909b76082055a61586af20b4dab674732ebd4b599eef080c9be
String digestHex = digester.digestHex("aaaaa");
```

> Java标准库的`java.security`包提供了一种标准机制，允许第三方提供商无缝接入。当引入`Bouncy Castle`库的jar后，Hutool会自动检测并接入。具体方法可见`SecureUtil.createMessageDigest`。
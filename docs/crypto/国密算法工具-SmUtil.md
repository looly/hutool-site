## 介绍
Hutool针对`Bouncy Castle`做了简化包装，用于实现国密算法中的SM2、SM3、SM4。

国密算法工具封装包括：

- 非对称加密和签名：SM2
- 摘要签名算法：SM3
- 对称加密：SM4

国密算法需要引入`Bouncy Castle`库的依赖。

## 使用

### 引入`Bouncy Castle`依赖

```xml
<dependency>
  <groupId>org.bouncycastle</groupId>
  <artifactId>bcprov-jdk15to18</artifactId>
  <version>1.66</version>
</dependency>
```

> 说明
> `bcprov-jdk15to18`的版本请前往Maven中央库搜索，查找对应JDK的最新版本。

### 非对称加密SM2

1. 使用随机生成的密钥对加密或解密

```java
String text = "我是一段测试aaaa";

SM2 sm2 = SmUtil.sm2();
// 公钥加密，私钥解密
String encryptStr = sm2.encryptBcd(text, KeyType.PublicKey);
String decryptStr = StrUtil.utf8Str(sm2.decryptFromBcd(encryptStr, KeyType.PrivateKey));
```

2. 使用自定义密钥对加密或解密

```java
String text = "我是一段测试aaaa";

KeyPair pair = SecureUtil.generateKeyPair("SM2");
byte[] privateKey = pair.getPrivate().getEncoded();
byte[] publicKey = pair.getPublic().getEncoded();

SM2 sm2 = SmUtil.sm2(privateKey, publicKey);
// 公钥加密，私钥解密
String encryptStr = sm2.encryptBcd(text, KeyType.PublicKey);
String decryptStr = StrUtil.utf8Str(sm2.decryptFromBcd(encryptStr, KeyType.PrivateKey));
```

3. SM2签名和验签

```java
String content = "我是Hanley.";
final SM2 sm2 = SmUtil.sm2();
String sign = sm2.signHex(HexUtil.encodeHexStr(content));

// true
boolean verify = sm2.verifyHex(HexUtil.encodeHexStr(content), sign);
```

当然，也可以自定义密钥对：

```java
String content = "我是Hanley.";
KeyPair pair = SecureUtil.generateKeyPair("SM2");
final SM2 sm2 = new SM2(pair.getPrivate(), pair.getPublic());

byte[] sign = sm2.sign(content.getBytes());

// true
boolean verify = sm2.verify(content.getBytes(), sign);
```

4. 使用SM2曲线点构建SM2

使用曲线点构建中的点生成和验证见：[https://i.goto327.top/CryptTools/SM2.aspx?tdsourcetag=s_pctim_aiomsg](https://i.goto327.top/CryptTools/SM2.aspx?tdsourcetag=s_pctim_aiomsg)

```java
String privateKeyHex = "FAB8BBE670FAE338C9E9382B9FB6485225C11A3ECB84C938F10F20A93B6215F0";
String x = "9EF573019D9A03B16B0BE44FC8A5B4E8E098F56034C97B312282DD0B4810AFC3";
String y = "CC759673ED0FC9B9DC7E6FA38F0E2B121E02654BF37EA6B63FAF2A0D6013EADF";

// 数据和ID此处使用16进制表示
String dataHex = "434477813974bf58f94bcf760833c2b40f77a5fc360485b0b9ed1bd9682edb45";
String idHex = "31323334353637383132333435363738";

final SM2 sm2 = new SM2(privateKeyHex, x, y);
final String sign = sm2.signHex(data, id);
// true
boolean verify = sm2.verifyHex(data, sign)
```

### 摘要加密算法SM3

```java
//结果为：136ce3c86e4ed909b76082055a61586af20b4dab674732ebd4b599eef080c9be
String digestHex = SmUtil.sm3("aaaaa");
```

### 对称加密SM4

```java
String content = "test中文";
SymmetricCrypto sm4 = SmUtil.sm4();

String encryptHex = sm4.encryptHex(content);
String decryptStr = sm4.decryptStr(encryptHex, CharsetUtil.CHARSET_UTF_8);
```
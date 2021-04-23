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
  <version>1.68</version>
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
String data = "434477813974bf58f94bcf760833c2b40f77a5fc360485b0b9ed1bd9682edb45";
String id = "31323334353637383132333435363738";

final SM2 sm2 = new SM2(privateKeyHex, x, y);
// 生成的签名是64位
sm2.usePlainEncoding();

final String sign = sm2.signHex(data, id);
// true
boolean verify = sm2.verifyHex(data, sign)
```

5. 使用私钥D值签名

```java
//需要签名的明文,得到明文对应的字节数组
byte[] dataBytes = "我是一段测试aaaa".getBytes();
//指定的私钥
String privateKeyHex = "1ebf8b341c695ee456fd1a41b82645724bc25d79935437d30e7e4b0a554baa5e";

// 此构造从5.5.9开始可使用
final SM2 sm2 = new SM2(privateKeyHex, null, null);
sm2.usePlainEncoding();
byte[] sign = sm2.sign(dataBytes, null);
```

6. 使用公钥Q值验证签名

```java
//指定的公钥
String publicKeyHex ="04db9629dd33ba568e9507add5df6587a0998361a03d3321948b448c653c2c1b7056434884ab6f3d1c529501f166a336e86f045cea10dffe58aa82ea13d725363";
//需要加密的明文,得到明文对应的字节数组
byte[] dataBytes = "我是一段测试aaaa".getBytes();
//签名值
String signHex ="2881346e038d2ed706ccdd025f2b1dafa7377d5cf090134b98756fafe084dddbcdba0ab00b5348ed48025195af3f1dda29e819bb66aa9d4d088050ff148482a";

final SM2 sm2 = new SM2(null, ECKeyUtil.toSm2PublicParams(publicKeyHex));
sm2.usePlainEncoding();

// true
boolean verify = sm2.verify(dataBytes, HexUtil.decodeHex(signHex));
```

7. 其他格式的密钥

在SM2算法中，密钥的格式分以下几种：

私钥：

- D值    一般为硬件直接生成的值
- PKCS#8 JDK默认生成的私钥格式
- PKCS#1 一般为OpenSSL生成的的EC密钥格式

公钥：

- Q值    一般为硬件直接生成的值
- X.509  JDK默认生成的公钥格式
- PKCS#1 一般为OpenSSL生成的的EC密钥格式

在新版本的Hutool中，SM2的构造方法对这几类的密钥都做了兼容，即用户无需关注密钥类型：


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
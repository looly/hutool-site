## 介绍

JWT签名算法比较多，主要分为非对称算法和对称算法，支持的算法定义在`SignAlgorithm`中。

### 对称签名

- HS256(HmacSHA256)
- HS384(HmacSHA384)
- HS512(HmacSHA512)

### 非对称签名

- RS256(SHA256withRSA)
- RS384(SHA384withRSA)
- RS512(SHA512withRSA)

- ES256(SHA256withECDSA)
- ES384(SHA384withECDSA)
- ES512(SHA512withECDSA)

### 依赖于BounyCastle的算法

- PS256(SHA256WithRSA/PSS)
- PS384(SHA384WithRSA/PSS)
- PS512(SHA512WithRSA/PSS)

## 使用

### 创建预定义算法签名器

`JWTSignerUtil`中预定义了一些算法的签名器的创建方法，如创建HS256的签名器：

```java
final JWTSigner signer = JWTSignerUtil.hs256("123456".getBytes());
JWT jwt = JWT.create().setSigner(signer);
```

### 创建自定义算法签名器

通过`JWTSignerUtil.createSigner`即可通过动态传入`algorithmId`创建对应的签名器，如我们如果需要实现`ps256`算法，则首先引入`bcprov-jdk15to18`包：

```xml
<dependency>
	<groupId>org.bouncycastle</groupId>
	<artifactId>bcprov-jdk15to18</artifactId>
	<version>1.69</version>
</dependency>
```

再创建对应签名器即可：

```java
String id = "ps256";
final JWTSigner signer = JWTSignerUtil.createSigner(id, KeyUtil.generateKeyPair(AlgorithmUtil.getAlgorithm(id)));

JWT jwt = JWT.create().setSigner(signer);
```

### 自行实现算法签名器

`JWTSigner`接口是一个通用的签名器接口，如果想实现自定义算法，实现此接口即可。
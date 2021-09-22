## 介绍

由于`JWT.verify`，只能验证JWT Token的签名是否有效，其他payload字段验证都可以使用`JWTValidator`完成。

## 使用

### 验证算法

算法的验证包括两个方面

1. 验证header中算法ID和提供的算法ID是否一致
2. 调用`JWT.verify`验证token是否正确

```java
// 创建JWT Token
final String token = JWT.create()
	.setNotBefore(DateUtil.date())
	.setKey("123456".getBytes())
	.sign();

// 验证算法
JWTValidator.of(token).validateAlgorithm(JWTSignerUtil.hs256("123456".getBytes()));
```

### 验证时间

对于时间类载荷，有单独的验证方法，主要包括：

- 生效时间（`JWTPayload#NOT_BEFORE`）不能晚于当前时间
- 失效时间（`JWTPayload#EXPIRES_AT`）不能早于当前时间
- 签发时间（`JWTPayload#ISSUED_AT`）不能晚于当前时间

一般时间线是：

(签发时间)---------(生效时间)---------(**当前时间**)---------(失效时间)

> 签发时间和生效时间一般没有前后要求，都早于当前时间即可。

```java
final String token = JWT.create()
	// 设置签发时间
	.setIssuedAt(DateUtil.date())
	.setKey("123456".getBytes())
	.sign();

// 由于只定义了签发时间，因此只检查签发时间
JWTValidator.of(token).validateDate(DateUtil.date());
```
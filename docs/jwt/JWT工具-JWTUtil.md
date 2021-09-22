## 介绍

我们可以通过`JWT`实现链式创建JWT对象或JWT字符串，Hutool同样提供了一些快捷方法封装在`JWTUtil`中。主要包括：

- JWT创建
- JWT解析
- JWT验证

## 使用

- JWT创建

```java
Map<String, Object> map = new HashMap<String, Object>() {
	private static final long serialVersionUID = 1L;
	{
		put("uid", Integer.parseInt("123"));
		put("expire_time", System.currentTimeMillis() + 1000 * 60 * 60 * 24 * 15);
	}
};

JWTUtil.createToken(map, "1234".getBytes());
```

- JWT解析

```java
String rightToken = "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9." +
	"eyJzdWIiOiIxMjM0NTY3ODkwIiwiYWRtaW4iOnRydWUsIm5hbWUiOiJsb29seSJ9." +
	"U2aQkC2THYV9L0fTN-yBBI7gmo5xhmvMhATtu8v0zEA";

final JWT jwt = JWTUtil.parseToken(rightToken);

jwt.getHeader(JWTHeader.TYPE);
jwt.getPayload("sub");
```

- JWT验证

```java
String token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9." +
	"eyJ1c2VyX25hbWUiOiJhZG1pbiIsInNjb3BlIjpbImFsbCJdLCJleHAiOjE2MjQwMDQ4MjIsInVzZXJJZCI6MSwiYXV0aG9yaXRpZXMiOlsiUk9MRV_op5LoibLkuozlj7ciLCJzeXNfbWVudV8xIiwiUk9MRV_op5LoibLkuIDlj7ciLCJzeXNfbWVudV8yIl0sImp0aSI6ImQ0YzVlYjgwLTA5ZTctNGU0ZC1hZTg3LTVkNGI5M2FhNmFiNiIsImNsaWVudF9pZCI6ImhhbmR5LXNob3AifQ." +
	"aixF1eKlAKS_k3ynFnStE7-IRGiD5YaqznvK2xEjBew";

JWTUtil.verify(token, "123456".getBytes());
```
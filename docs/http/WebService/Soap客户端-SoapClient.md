## 由来
在接口对接当中，WebService接口占有着很大份额，而我们为了使用这些接口，不得不引入类似Axis等库来实现接口请求。

现在有了Hutool，就可以在无任何依赖的情况下，实现简便的WebService请求。

## 使用

1. 使用[SoapUI](https://www.soapui.org/)解析WSDL地址，找到WebService方法和参数。

我们得到的XML模板为：

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:web="http://WebXml.com.cn/">
   <soapenv:Header/>
   <soapenv:Body>
      <web:getCountryCityByIp>
         <!--Optional:-->
         <web:theIpAddress>?</web:theIpAddress>
      </web:getCountryCityByIp>
   </soapenv:Body>
</soapenv:Envelope>
```

2. 按照SoapUI中的相应内容构建SOAP请求。

我们知道：

1. 方法名为：`web:getCountryCityByIp`
2. 参数只有一个，为:`web:theIpAddress`
3. 定义了一个命名空间，前缀为`web`，URI为`http://WebXml.com.cn/`

这样我们就能构建相应SOAP请求：

```java
// 新建客户端
SoapClient client = SoapClient.create("http://www.webxml.com.cn/WebServices/IpAddressSearchWebService.asmx")
    // 设置要请求的方法，此接口方法前缀为web，传入对应的命名空间
    .setMethod("web:getCountryCityByIp", "http://WebXml.com.cn/")
    // 设置参数，此处自动添加方法的前缀：web
    .setParam("theIpAddress", "218.21.240.106");

    // 发送请求，参数true表示返回一个格式化后的XML内容
    // 返回内容为XML字符串，可以配合XmlUtil解析这个响应
    Console.log(client.send(true));
```

## 扩展

### 查看生成的请求XML

调用`SoapClient`对象的`getMsgStr`方法可以查看生成的XML，以检查是否与SoapUI生成的一致。

```java
SoapClient client = ...;
Console.log(client.getMsgStr(true));
```

### 多参数或复杂参数

对于请求体是列表参数或多参数的情况，如：

```xml
<web:method>
  <arg0>
    <fd1>aaa</fd1>
    <fd2>bbb</fd2>
  </arg0>
</web:method>
```

这类请求可以借助`addChildElement`完成。

```java
SoapClient client = SoapClient.create("https://hutool.cn/WebServices/test.asmx")
		.setMethod("web:method", "http://hutool.cn/")
		SOAPElement arg0 = client.getMethodEle().addChildElement("arg0");
		arg0.addChildElement("fdSource").setValue("?");
		arg0.addChildElement("fdTemplated").setValue("?");
```

> 详细的问题解答见：https://gitee.com/dromara/hutool/issues/I4QL1V
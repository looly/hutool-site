## 由来

在JDK中，我们可以借助`URL`对象完成URL的格式化，但是无法完成一些特殊URL的解析和处理，例如编码过的URL、不标准的路径和参数。在旧版本的hutool中，URL的规范完全靠字符串的替换来完成，不但效率低，而且处理过程及其复杂。于是在5.3.1之后，加入了UrlBuilder类，拆分URL的各个部分，分别处理和格式化，完成URL的规范。

按照[Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)的标准定义，URL的结构如下：

- [scheme:]scheme-specific-part[#fragment]

- [scheme:][//authority][path][?query][#fragment]

- [scheme:][//host:port][path][?query][#fragment]


按照这个格式，UrlBuilder将URL分成scheme、host、port、path、query、fragment部分，其中path和query较为复杂，又使用`UrlPath`和`UrlQuery`分别封装。

## 使用

相比`URL`对象，UrlBuilder更加人性化，例如：

```java
URL url = new URL("www.hutool.cn")
```

此时会报`java.net.MalformedURLException: no protocol`的错误，而使用UrlBuilder则会有默认协议：

```java
// 输出 http://www.hutool.cn/
String buildUrl = UrlBuilder.create().setHost("www.hutool.cn").build();
```

### 完整构建

```java
// https://www.hutool.cn/aaa/bbb?ie=UTF-8&wd=test
String buildUrl = UrlBuilder.create()
	.setScheme("https")
	.setHost("www.hutool.cn")
	.addPath("/aaa").addPath("bbb")
	.addQuery("ie", "UTF-8")
	.addQuery("wd", "test")
	.build();
```

### 中文编码

当参数中有中文时，自动编码中文，默认UTF-8编码，也可以调用`setCharset`方法自定义编码。

```java
// https://www.hutool.cn/s?ie=UTF-8&ie=GBK&wd=%E6%B5%8B%E8%AF%95
String buildUrl = UrlBuilder.create()
	.setScheme("https")
	.setHost("www.hutool.cn")
	.addPath("/s")
	.addQuery("ie", "UTF-8")
	.addQuery("ie", "GBK")
	.addQuery("wd", "测试")
	.build();
```

### 解析

当有一个URL字符串时，可以使用`of`方法解析：

```java
UrlBuilder builder = UrlBuilder.ofHttp("www.hutool.cn/aaa/bbb/?a=张三&b=%e6%9d%8e%e5%9b%9b#frag1", CharsetUtil.CHARSET_UTF_8);

// 输出张三
Console.log(builder.getQuery().get("a"));
// 输出李四
Console.log(builder.getQuery().get("b"));
```

我们发现这个例子中，原URL中的参数a是没有编码的，b是编码过的，当用户提供此类混合URL时，Hutool可以很好的识别并全部decode，当然，调用build()之后，会全部再encode。

### 特殊URL解析

有时候URL中会存在`&amp;`这种分隔符，谷歌浏览器会将此字符串转换为`&`使用，Hutool中也同样如此：

```java
String urlStr = "https://mp.weixin.qq.com/s?__biz=MzI5NjkyNTIxMg==&amp;mid=100000465&amp;idx=1";
UrlBuilder builder = UrlBuilder.ofHttp(urlStr, CharsetUtil.CHARSET_UTF_8);

// https://mp.weixin.qq.com/s?__biz=MzI5NjkyNTIxMg==&mid=100000465&idx=1
Console.log(builder.build());
```

> UrlBuilder主要应用于http模块，在构建HttpRequest时，用户传入的URL五花八门，为了做大最好的适应性，减少用户对URL的处理，使用UrlBuilder完成URL的规范化。
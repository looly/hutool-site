## 介绍

拼音工具类在旧版本的Hutool中在core包中，但是发现自己实现相关功能需要庞大的字典，放在core包中便是累赘。

于是为了方便，Hutool封装了拼音的门面，用于兼容以下拼音库:

1. TinyPinyin
2. JPinyin
3. Pinyin4j

和其它门面模块类似，采用SPI方式识别所用的库。例如你想用Pinyin4j，只需引入jar，Hutool即可自动识别。

## 使用

### 引入库

以下为Hutool支持的拼音库的pom坐标，你可以选择任意一个引入项目中，如果引入多个，Hutool会按照以上顺序选择第一个使用。

```xml
<dependency>
	<groupId>io.github.biezhi</groupId>
	<artifactId>TinyPinyin</artifactId>
	<version>2.0.3.RELEASE</version>
</dependency>
```

```xml
<dependency>
	<groupId>com.belerweb</groupId>
	<artifactId>pinyin4j</artifactId>
	<version>2.5.1</version>
</dependency>
```

```xml
<dependency>
	<groupId>com.github.stuxuhai</groupId>
	<artifactId>jpinyin</artifactId>
	<version>1.1.8</version>
</dependency>
```

### 使用

1. 获取拼音

```java
// "ni hao"
String pinyin = PinyinUtil.getPinyin("你好", " ");
```

这里定义的分隔符为空格，你也可以按照需求自定义分隔符，亦或者使用""无分隔符。

2. 获取拼音首字母

```java
// "h, s, d, y, g"
String result = PinyinUtil.getFirstLetter("H是第一个", ", ");
```

3. 自定义拼音库（拼音引擎）

```java
Pinyin4jEngine engine = new Pinyin4jEngine();

// "ni hao h"
String pinyin = engine.getPinyin("你好h", " ");
```
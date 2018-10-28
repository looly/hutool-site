二维码工具-QrCodeUtil
===

## 由来
由于大家对二维码的需求较多，对于二维码的生成和解析我认为应该作为简单的工具存在于Hutool中。考虑到自行实现的难度，因此Hutool针对被广泛接受的的[zxing](https://github.com/zxing/zxing)库进行封装。而由于涉及第三方包，因此归类到extra模块中。

## 使用

### 引入zxing
考虑到Hutool的非强制依赖性，因此zxing需要用户自行引入：

```xml
<dependency>
	<groupId>com.google.zxing</groupId>
	<artifactId>core</artifactId>
	<version>3.3.1</version>
</dependency>
```

> 说明
> zxing-3.3.1是此文档编写时的最新版本，理论上你引入的版本应比这个版本新。

### 生成二维码

在此我们将Hutool主页的url生成为二维码，微信扫一扫可以看到H5主页哦：

```java
// 生成指定url对应的二维码到文件，宽和高都是300像素
QrCodeUtil.generate("http://hutool.cn/", 300, 300, FileUtil.file("d:/qrcode.jpg"));
```

效果qrcode.jpg：
![](https://static.oschina.net/uploads/img/201801/23203646_3TUp.jpg)

### 自定义参数（since 4.1.2）

通过QrConfig配置对象可以自定义二维码的更对参数，例如长、宽、二维码的颜色、背景颜色、边距等参数，使用方法如下：

```
QrConfig config = new QrConfig(300, 300);
// 设置边距，既二维码和背景之间的边距
config.setMargin(3);
// 设置前景色，既二维码颜色（青色）
config.setForeColor(Color.CYAN.getRGB());
// 设置背景色（灰色）
config.setBackColor(Color.GRAY.getRGB());
// 生成二维码到文件，也可以到流
QrCodeUtil.generate("http://hutool.cn/", config, FileUtil.file("e:/qrcode.jpg"));
```

效果qrcode.jpg:
![](https://static.oschina.net/uploads/img/201807/15113057_Zc8G.jpg)

### 识别二维码

```java
// decode -> "http://hutool.cn/"
String decode = QrCodeUtil.decode(FileUtil.file("d:/qrcode.jpg"));
```


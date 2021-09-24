## 介绍

虽然Hutool基于JDK提供了`ZipUtil`用于压缩或解压ZIP相关文件，但是对于7zip、tar等格式的压缩依旧无法处理，于是基于`commons-compress`做了进一步封装：`CompressUtil`。

此工具支持的格式有：

对于流式压缩支持：

- GZIP
- BZIP2
- XZ
- XZ
- PACK200
- SNAPPY_FRAMED
- LZ4_BLOCK
- LZ4_FRAMED
- ZSTANDARD
- DEFLATE

对于归档文件支持：

- AR
- CPIO
- JAR
- TAR
- ZIP
- 7z

对于归档文件，Hutool提供了两个通用接口：

- Archiver  数据归档，提供打包工作，如增加文件到压缩包等
- Extractor 归档数据解包，用于解压或者提取压缩文件

## 使用

首先引入`commons-compress`

```xml
<dependency>
	<groupId>org.apache.commons</groupId>
	<artifactId>commons-compress</artifactId>
	<version>1.21</version>
</dependency>
```

### 压缩文件

我们以7Zip为例：

```java
final File file = FileUtil.file("d:/test/compress/test.7z");
CompressUtil.createArchiver(CharsetUtil.CHARSET_UTF_8, ArchiveStreamFactory.SEVEN_Z, file)
	.add(FileUtil.file("d:/test/someFiles"));
	.finish()
	.close();
```

其中`ArchiveStreamFactory.SEVEN_Z`就是自定义的压缩格式，可以自行选择

add方法同时支持文件或目录，多个文件目录多次调用add方法即可。

有时候我们不想把目录下所有的文件放到压缩包，这时候可以使用add方法的第二个参数`Filter`，此接口用于过滤不需要加入的文件。

```java
CompressUtil.createArchiver(CharsetUtil.CHARSET_UTF_8, ArchiveStreamFactory.SEVEN_Z, zipFile)
	.add(FileUtil.file("d:/Java/apache-maven-3.6.3"), (file)->{
		if("invalid".equals(file.getName())){
			return false;
		}
		return true;
	})
	.finish().close();
```

### 解压文件

我们以7Zip为例：

```java
Extractor extractor = 	CompressUtil.createExtractor(
		CharsetUtil.defaultCharset(),
		FileUtil.file("d:/test/compress/test.7z"));

extractor.extract(FileUtil.file("d:/test/compress/test2/"));
```
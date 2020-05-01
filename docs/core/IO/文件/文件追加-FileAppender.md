## 由来

顾名思义，`FileAppender`类表示文件追加器。此对象持有一个一个文件，在内存中积累一定量的数据后统一追加到文件，此类只有在写入文件时打开文件，并在写入结束后关闭之。因此此类不需要关闭。

在调用append方法后会缓存于内存，只有超过容量后才会一次性写入文件，因此内存中随时有剩余未写入文件的内容，在最后必须调用flush方法将剩余内容刷入文件。

也就是说，这是一个支持缓存的文件内容追加器。此类主要用于类似于日志写出这类需求所用。

## 使用

```java
FileAppender appender = new FileAppender(file, 16, true);
appender.append("123");
appender.append("abc");
appender.append("xyz");

appender.flush();
appender.toString();
```
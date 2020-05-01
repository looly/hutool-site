## 由来

有时候我们要启动一个线程实时“监控”文件的变化，比如有新内容写出到文件时，我们可以及时打印出来，这个功能非常类似于Linux下的`tail -f`命令。

## 使用

```java
Tailer tailer = new Tailer(FileUtil.file("f:/test/test.log"), Tailer.CONSOLE_HANDLER, 2);
tailer.start();
```

其中`Tailer.CONSOLE_HANDLER`表示文件新增内容默认输出到控制台。

```java
/**
 * 命令行打印的行处理器
 * 
 * @author looly
 * @since 4.5.2
 */
public static class ConsoleLineHandler implements LineHandler {
	@Override
	public void handle(String line) {
		Console.log(line);
	}
}
```

我们也可以实现自己的LineHandler来处理每一行数据。

> 注意
> 此方法会阻塞当前线程
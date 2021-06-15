## 介绍

针对异常封装，例如包装为`RuntimeException`。

## 方法

### 包装异常

假设系统抛出一个非Runtime异常，我们需要包装为Runtime异常，那么：

```java
IORuntimeException e = ExceptionUtil.wrap(new IOException(), IORuntimeException.class);
```

### 获取入口方法

```java
StackTraceElement ele = ExceptionUtil.getRootStackElement();
// main
ele.getMethodName();
```

### 异常转换

如果我们想把异常转换指定异常为来自或者包含指定异常，那么：

```java
IOException ioException = new IOException();
IllegalArgumentException argumentException = new IllegalArgumentException(ioException);

IOException ioException1 = ExceptionUtil.convertFromOrSuppressedThrowable(argumentException, IOException.class, true);
```

### 其他方法

- `getMessage` 获得完整消息，包括异常名
- `wrapRuntime` 使用运行时异常包装编译异常
- `getCausedBy` 获取由指定异常类引起的异常
- `isCausedBy` 判断是否由指定异常类引起
- `stacktraceToString` 堆栈转为完整字符串

其它方法见API文档：

[https://apidoc.gitee.com/dromara/hutool/cn/hutool/core/exceptions/ExceptionUtil.html](https://apidoc.gitee.com/dromara/hutool/cn/hutool/core/exceptions/ExceptionUtil.html)


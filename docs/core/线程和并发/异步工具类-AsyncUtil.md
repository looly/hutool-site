## 由来

在`JDK8`中，提供了`CompletableFuture`进行异步执行

使用场景如异步调用微服务、异步查询数据库、异步运算大量数据等

## 方法

### AsyncUtil.waitAll

等待所有任务执行完毕

### AsyncUtil.waitAny

等待任意一个任务执行完毕

### AsyncUtil.get

获取异步任务结果
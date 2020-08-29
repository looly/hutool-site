## 由来

在JDK中，提供了`Executors`用于创建自定义的线程池对象`ExecutorService`，但是考虑到线程池中存在众多概念，这些概念通过不同的搭配实现灵活的线程管理策略，单独使用`Executors`无法满足需求，构建了`ExecutorBuilder`。

### 概念

- `corePoolSize` 初始池大小
- `maxPoolSize`  最大池大小（允许同时执行的最大线程数）
- `workQueue`    队列，用于存在未执行的线程
- `handler`      当线程阻塞（block）时的异常处理器，所谓线程阻塞即线程池和等待队列已满，无法处理线程时采取的策略

### 线程池对待线程的策略

1. 如果池中任务数 < corePoolSize     -> 放入立即执行
2. 如果池中任务数 > corePoolSize     -> 放入队列等待
3. 队列满                            -> 新建线程立即执行
4. 执行中的线程 > maxPoolSize        -> 触发handler（RejectedExecutionHandler）异常

### workQueue线程池策略

- `SynchronousQueue` 它将任务直接提交给线程而不保持它们。当运行线程小于`maxPoolSize`时会创建新线程，否则触发异常策略
- `LinkedBlockingQueue` 默认无界队列，当运行线程大于`corePoolSize`时始终放入此队列，此时`maxPoolSize`无效。当构造LinkedBlockingQueue对象时传入参数，变为有界队列，队列满时，运行线程小于`maxPoolSize`时会创建新线程，否则触发异常策略
- `ArrayBlockingQueue` 有界队列，相对无界队列有利于控制队列大小，队列满时，运行线程小于`maxPoolSize`时会创建新线程，否则触发异常策略

## 使用

1. 默认线程池

策略如下：

- 初始线程数为corePoolSize指定的大小
- 没有最大线程数限制
- 默认使用LinkedBlockingQueue，默认队列大小为1024（最大等待数1024）
- 当运行线程大于corePoolSize放入队列，队列满后抛出异常

```java
ExecutorService executor = ExecutorBuilder builder = ExecutorBuilder.create()..build();
```

2. 单线程线程池

- 初始线程数为 1
- 最大线程数为 1
- 默认使用LinkedBlockingQueue，默认队列大小为1024
- 同时只允许一个线程工作，剩余放入队列等待，等待数超过1024报错

```java
ExecutorService executor = ExecutorBuilder.create()//
	.setCorePoolSize(1)//
	.setMaxPoolSize(1)//
	.setKeepAliveTime(0)//
	.build();
```

3. 更多选项的线程池

- 初始5个线程
- 最大10个线程
- 有界等待队列，最大等待数是100

```java
ExecutorService executor = ExecutorBuilder.create()
	.setCorePoolSize(5)
	.setMaxPoolSize(10)
	.setWorkQueue(new LinkedBlockingQueue<>(100))
	.build();
```

3. 特殊策略的线程池

- 初始5个线程
- 最大10个线程
- 它将任务直接提交给线程而不保持它们。当运行线程小于maxPoolSize时会创建新线程，否则触发异常策略

```java
ExecutorService executor = ExecutorBuilder.create()
	.setCorePoolSize(5)
	.setMaxPoolSize(10)
	.useSynchronousQueue()
	.build();
```
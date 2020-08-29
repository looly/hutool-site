## 由来

很多时候，我们需要简单模拟N个线程调用某个业务测试其并发状况，于是Hutool提供了一个简单的并发测试类——ConcurrencyTester。

## 使用

```java
ConcurrencyTester tester = ThreadUtil.concurrencyTest(100, () -> {
	// 测试的逻辑内容
	long delay = RandomUtil.randomLong(100, 1000);
	ThreadUtil.sleep(delay);
	Console.log("{} test finished, delay: {}", Thread.currentThread().getName(), delay);
});

// 获取总的执行时间，单位毫秒
Console.log(tester.getInterval());
```

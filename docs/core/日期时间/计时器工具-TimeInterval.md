## 介绍

Hutool通过封装`TimeInterval`实现计时器功能，即可以计算方法或过程执行的时间。

`TimeInterval`支持分组计时，方便对比时间。

## 使用

```java
TimeInterval timer = DateUtil.timer();

//---------------------------------
//-------这是执行过程
//---------------------------------

timer.interval();//花费毫秒数
timer.intervalRestart();//返回花费时间，并重置开始时间
timer.intervalMinute();//花费分钟数
```

也可以实现分组计时：

```java
final TimeInterval timer = new TimeInterval();

// 分组1
timer.start("1");
ThreadUtil.sleep(800);

// 分组2
timer.start("2");
ThreadUtil.sleep(900);

Console.log("Timer 1 took {} ms", timer.intervalMs("1"));
Console.log("Timer 2 took {} ms", timer.intervalMs("2"));
```
## 概述

`Oshi`是Java的免费基于JNA的操作系统和硬件信息库，Github地址是：https://github.com/oshi/oshi

它的优点是不需要安装任何其他本机库，并且旨在提供一种跨平台的实现来检索系统信息，例如操作系统版本，进程，内存和CPU使用率，磁盘和分区，设备，传感器等。

这个库可以监测的内容包括：

1. 计算机系统和固件，底板
2. 操作系统和版本/内部版本
3. 物理（核心）和逻辑（超线程）CPU，处理器组，NUMA节点
4. 系统和每个处理器的负载百分比和滴答计数器
5. CPU正常运行时间，进程和线程
6. 进程正常运行时间，CPU，内存使用率，用户/组，命令行
7. 已使用/可用的物理和虚拟内存
8. 挂载的文件系统（类型，可用空间和总空间）
9. 磁盘驱动器（型号，序列号，大小）和分区
10. 网络接口（IP，带宽输入/输出）
11. 电池状态（电量百分比，剩余时间，电量使用情况统计信息）
12. 连接的显示器（带有EDID信息）
13. USB设备
14. 传感器（温度，风扇速度，电压）

也就是说配合一个前端界面，完全可以搞定系统监控了。

## 使用

先引入Oshi库：

```xml
<dependency>
	<groupId>com.github.oshi</groupId>
	<artifactId>oshi-core</artifactId>
	<version>5.6.1</version>
</dependency>
```

然后可以调用相关API获取相关信息。

例如我们像获取内存总量：

```java
long total = OshiUtil.getMemory().getTotal();
```

我们也可以获取CPU的一些信息：

```java
CpuInfo cpuInfo = OshiUtil.getCpuInfo();
Console.log(cpuInfo);
```

```
CpuInfo{cpu核心数=12, CPU总的使用率=12595.0, CPU系统使用率=1.74, CPU用户使用率=6.69, CPU当前等待率=0.0, CPU当前空闲率=91.57, CPU利用率=8.43, CPU型号信息='AMD Ryzen 5 4600U with Radeon Graphics         
 1 physical CPU package(s)
 6 physical CPU core(s)
 12 logical CPU(s)
Identifier: AuthenticAMD Family 23 Model 96 Stepping 1
ProcessorID: xxxxxxxxx
Microarchitecture: unknown'}
```
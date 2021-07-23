## 说明
`RandomUtil`主要针对JDK中`Random`对象做封装，严格来说，Java产生的随机数都是伪随机数，因此Hutool封装后产生的随机结果也是伪随机结果。不过这种随机结果对于大多数情况已经够用。

## 使用

- `RandomUtil.randomInt` 获得指定范围内的随机数

例如我们想产生一个[10, 100)的随机数，则：
```java
int c = RandomUtil.randomInt(10, 100);
```

- `RandomUtil.randomBytes` 随机bytes，一般用于密码或者salt生成

```java
byte[] c = RandomUtil.randomBytes(10);
```

- `RandomUtil.randomEle` 随机获得列表中的元素
- `RandomUtil.randomEleSet` 随机获得列表中的一定量的不重复元素，返回Set

```java
Set<Integer> set = RandomUtil.randomEleSet(CollUtil.newArrayList(1, 2, 3, 4, 5, 6), 2);
```

- `RandomUtil.randomString` 获得一个随机的字符串（只包含数字和字符）
- `RandomUtil.randomNumbers` 获得一个只包含数字的字符串
- `RandomUtil.weightRandom` 权重随机生成器，传入带权重的对象，然后根据权重随机获取对象

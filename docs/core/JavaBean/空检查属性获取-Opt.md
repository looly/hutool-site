## 介绍

在嵌套对象的属性获取中，由于子对象无法得知是否为`null`，每次获取属性都要检查属性兑现是否为null，使得代码会变得特备臃肿，因此使用`Opt`来优雅的链式获取属性对象值。

> 声明：此类的作者：[阿超](https://gitee.com/VampireAchao) ，PR来自：https://gitee.com/dromara/hutool/pulls/426

## 使用

我们先定义一个嵌套的Bean：

```java
// Lombok注解
@Data
public static class User {
	private String name;
	private String gender;
	private School school;
	
	@Data
	public static class School {
		private String name;
		private String address;
	}
}
```

假设我们想获取`address`属性，则：

```java
User user = new User();
user.setName("hello");

// null
String addressValue = Opt.ofNullable(user)
		.map(User::getSchool)
		.map(User.School::getAddress).get();
```

由于school对象的值为`null`，一般直接获取会报空指针，使用`Opt`即可避免判断。

- ofBlankAble函数基于ofNullable的逻辑下，额外进行了空字符串判断
```java
// ofBlankAble相对于ofNullable考虑了字符串为空串的情况
String hutool = Opt.ofBlankAble("").orElse("hutool");
Assert.equals("hutool", hutool);
```
- 原版Optional有区别的是，get不会抛出NoSuchElementException
- 如果想使用原版Optional中的get这样，获取一个一定不为空的值，则应该使用orElseThrow
```java
// 和原版Optional有区别的是，get不会抛出NoSuchElementException
// 如果想使用原版Optional中的get这样，获取一个一定不为空的值，则应该使用orElseThrow
Object opt = Opt.ofNullable(null).get();
Assert.isNull(isEmpty);
```
- 这是参考了jdk11 Optional中的新函数isEmpty，用于判断不存在值的情况
```java
// 这是参考了jdk11 Optional中的新函数
// 判断包裹内元素是否为空，注意并没有判断空字符串的情况
boolean isEmpty = Opt.empty().isEmpty();
Assert.isTrue(isEmpty);
```
- 灵感来源于jdk9 Optional中的新函数ifPresentOrElse，用于 存在值时执行某些操作，不存在值时执行另一个操作，支持链式编程
```java
// 灵感来源于jdk9 Optional中的新函数ifPresentOrElse
// 存在就打印对应的值，不存在则用{@code System.err.println}打印另一句字符串
Opt.ofNullable("Hello Hutool!").ifPresentOrElse(System.out::println, () -> System.err.println("Ops!Something is wrong!"));
Opt.empty().ifPresentOrElse(System.out::println, () -> System.err.println("Ops!Something is wrong!"));
```
- 新增了peek函数，相当于ifPresent的链式调用（个人常用）
```java
User user = new User();
// 相当于ifPresent的链式调用
Opt.ofNullable("hutool").peek(user::setUsername).peek(user::setNickname);
Assert.equals("hutool", user.getNickname());
Assert.equals("hutool", user.getUsername());

// 注意，传入的lambda中，对包裹内的元素执行赋值操作并不会影响到原来的元素
String name = Opt.ofNullable("hutool").peek(username -> username = "123").peek(username -> username = "456").get();
Assert.equals("hutool", name);
```
- 灵感来源于jdk11 Optional中的新函数or，用于值不存在时，用别的Opt代替
```java
// 灵感来源于jdk11 Optional中的新函数or
// 给一个替代的Opt
String str = Opt.<String>ofNullable(null).or(() -> Opt.ofNullable("Hello hutool!")).map(String::toUpperCase).orElseThrow();
Assert.equals("HELLO HUTOOL!", str);

User user = User.builder().username("hutool").build();
Opt<User> userOpt = Opt.of(user);
// 获取昵称，获取不到则获取用户名
String name = userOpt.map(User::getNickname).or(() -> userOpt.map(User::getUsername)).get();
Assert.equals("hutool", name);
```
- 对orElseThrow进行了重载，支持 双冒号+自定义提示语 写法，比原来的
```java
orElseThrow(() -> new IllegalStateException("Ops!Something is wrong!"))
```
更加优雅,修改后写法为：
```java
orElseThrow(IllegalStateException::new, "Ops!Something is wrong!")
```

## 学习：

经常有朋友问我，你这个`Opt`，参数怎么都是一些`lambda`，我怎么知道对应的`lambda`怎么写呢？

这函数式编程，真是一件美事啊~

对于这种情况，我们依靠我们强大的`idea`即可

例如此处我写到这里写不会了

```java
User user = new User();
// idea提示下方参数，如果没显示，光标放到括号里按ctrl+p主动呼出            
         |Function<? super User,?> mapper|
Opt.ofNullable(user).map()
```

这里`idea`为我们提示了参数类型，可这个`Function`我也不知道它是个什么

实际上，我们`new`一个就好了

```java
Opt.ofNullable(user).map(new Fun)
                            |Function<User, Object>{...} (java.util.function)   |  <-戳我
                            |Func<P,R> cn.hutool.core.lang.func                 |
```

这里`idea`提示了剩下的代码，我们选`Function`就行了，接下来如下：

```java
Opt.ofNullable(user).map(new Function<User, Object>() {
})
```

此处开始编译报错了，不要着急，我们这里根据具体操作选取返回值

例如我这里是想判断`user`是否为空，不为空时调用`getSchool`，从而获取其中的返回值`String`类型的`school`

我们就如下写法，将第二个泛型，也就是象征返回值的泛型改为`String`：

```java
Opt.ofNullable(user).map(new Function<User, String>() {
})
```

然后我们使用`idea`的修复所有，默认快捷键`alt`+回车

```java
Opt.ofNullable(user).map(new Function<User, String>() {
})                                                | 💡 Implement methods                  |  <-选我
                                                  | ✍  Introduce local variable          |
                                                  | ↩  Rollback changes in current line   |
```

选择第一个`Implement methods`即可，这时候弹出一个框，提示让你选择你想要实现的方法

这里就选择我们的`apply`方法吧，按下一个回车就可以了，或者点击选中`apply`，再按一下`OK`按钮

```java
    ||IJ| Select Methods to Implement                        X |
    |                                                          |
    | 👇  ©  |  ↹  ↸                                          |
    | -------------------------------------------------------- |
    | | java.util.function.Function                            |
    | | ⒨ 🔓 apply(t:T):R                                     |      <-选我选我
    | | ⒨ 🔓 compose(before:Function<? super V,? extents T):Fu|
    | | ⒨ 🔓 andThen(after:Function<? super R,? extends V>):Fu|
    | |                                                        |
    | | ========================================               |                                        
    | -------------------------------------------------------- |
    |  ☐ Copy JavaDoc                                          |
    |  ☑ Insert @Override               |  OK  |  | CANCEL |   |     <-选完点我点我
```

此时此刻，代码变成了这样子

```java
Opt.ofNullable(user).map(new Function<User, String>() {
    @Override
    public String apply(User user) {
        return null;
    }
})
```

这里重写的方法里面就写你自己的逻辑(别忘了补全后面的分号)

```java
Opt.ofNullable(user).map(new Function<User, String>() {
    @Override
    public String apply(User user) {
        return user.getSchool();
    }
});
```

我们可以看到，上边的`new Function<User, String>()`变成了灰色

我们在它上面按一下`alt`+`enter`(回车)

```java
Opt.ofNullable(user).map(new Function<User, String>() {
    @Override                              | 💡 Replace with lambda             > |  <-选我啦
    public String apply(User user) {       | 💡 Replace with method reference   > |
        return user.getSchool();           | 💎 balabala...                     > |
    }
});
```

选择第一个`Replace with lambda`，就会自动缩写为`lambda`啦

```java
Opt.ofNullable(user).map(user1 -> user1.getSchool());
```

如果选择第二个，则会缩写为我们双冒号格式

```java
Opt.ofNullable(user).map(User::getSchool);
```

看，是不是很简单！
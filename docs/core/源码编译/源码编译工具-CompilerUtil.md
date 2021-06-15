## 介绍

JDK提供了`JavaCompiler`用于动态编译java源码文件，然后通过类加载器加载，这种动态编译可以让Java有动态脚本的特性，Hutool针对此封装了对应工具。

## 使用

首先我们将编译需要依赖的class文件和jar文件打成一个包：

```java
// 依赖A，编译B和C
final File libFile = ZipUtil.zip(FileUtil.file("lib.jar"),
		new String[]{"a/A.class", "a/A$1.class", "a/A$InnerClass.class"},
		new InputStream[]{
				FileUtil.getInputStream("test-compile/a/A.class"),
				FileUtil.getInputStream("test-compile/a/A$1.class"),
				FileUtil.getInputStream("test-compile/a/A$InnerClass.class")
		});
```

开始编译：

```java
final ClassLoader classLoader = CompilerUtil.getCompiler(null)
	// 被编译的源码文件
	.addSource(FileUtil.file("test-compile/b/B.java"))
	// 被编译的源码字符串
	.addSource("c.C", FileUtil.readUtf8String("test-compile/c/C.java"))
	// 编译依赖的库
	.addLibrary(libFile)
	.compile();
```

加载编译好的类：

```java
final Class<?> clazz = classLoader.loadClass("c.C");
// 实例化对象c
Object obj = ReflectUtil.newInstance(clazz);
```
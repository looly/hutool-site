## 介绍

RotN（rotate by N places），回转N位密码，是一种简易的替换式密码，也是过去在古罗马开发的凯撒加密的一种变体。

## 使用

以Rot-13为例：

```java
String str = "1f2e9df6131b480b9fdddc633cf24996";

// 4s5r2qs9464o713o2sqqqp966ps57229
String encode13 = Rot.encode13(str);

// 解码
String decode13 = Rot.decode13(encode13);
```
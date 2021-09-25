## 介绍

摩尔斯电码也被称作摩斯密码，是一种时通时断的信号代码，通过不同的排列顺序来表达不同的英文字母、数字和标点符号。

摩尔斯电码是由点dot（.）划dash（-）这两种符号所组成的。

## 实现

### 编码

```java
final Morse morseCoder = new Morse();

String text = "Hello World!";

// ...././.-../.-../---/-...../.--/---/.-./.-../-../-.-.--/
morseCoder.encode(text);
```

### 解码

```java
String text = "你好，世界！";

// -..----.--...../-.--..-.-----.-/--------....--../-..---....-.--./---.-.-.-..--../--------.......-/
String morse = morseCoder.encode(text);

morseCoder.decode(morse);
```
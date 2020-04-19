Base62编码解码-Base62
===

## 介绍

Base62编码是由10个数字、26个大写英文字母和26个小写英文字母组成，多用于安全领域和短URL生成。

## 使用

```java
String a = "伦家是一个非常长的字符串66";

// 17vKU8W4JMG8dQF8lk9VNnkdMOeWn4rJMva6F0XsLrrT53iKBnqo
String encode = Base62.encode(a);

// 还原为a
String decodeStr = Base62.decodeStr(encode);
```


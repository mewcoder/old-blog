---
layout:     post
title:      Java String类
subtitle:   String类 
date:       2019-06-17
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Java
typora-root-url: ..
---

## String类

### ☆面试题：String str1 = 'A'与 String str2 = new String("A")区别？

- 当执行第1句话的时候，会在常量池中添加一个新的A字符，str1指向常量池的A
- 当执行第2句话的时候，因为有new操作符，所以会在堆空间新开辟一块空间用来存储新的String对象，因为此时常量池中已经有了A字符，所以堆中的String对象指向常量池中的A，而str2则指向堆空间中的String对象。

```java
String a = "abc";
String b = "abc";
//a==b 的结果是true
//String常量存放在常量池中，Java虚拟出于优化的考虑，会让内容一致的对象共享内存块。

Integer i1 = 10;
Integer i2 = 10;
//i1==i2 的结果true 两个常量对象共享了一块内存空间

String b1 = new String("abc")
//b==b1 false 常量池和堆空间 不可能一致
String b2 = new String("abc")    
//b1==b2 false 内存地址不相同
    
String c= a;
String d = c +"bc";//变量 d==a false
String e = "a"+"bc"//常量 e==a true
    
```

### ☆面试题：String的“不可变性”

![preview](/img/assets_2019/46c03ae5abf6111879423f38375207cc_hd.jpg)

> 一旦一个 String 对象在内存（堆）中被创建出来，它就无法被修改（因为 String 类的所有成员变量都是 private，并且没有提供 public 的 set 方法来修改这些值。此外成员变量都是 final 的，这就意味着一旦初始化就无法修改）。特别要注意的是，String 类的所有方法都没有改变字符串本身的值，都是返回了一个新的对象
>
> 

**String使用注意事项：**

- `String  a = "123";`定义的是常量,`String b1 = new String("123");`定义的是变量。尽量使用常量
- 如果需要一个可修改的字符串，应该使用 StringBuffer 或者 StringBuilder。否则会有大量时间浪费在垃圾回收上，因为每次试图修改都有新的 String 对象被创建出来。
- 如果修改要采用s=s.replace('1','2');



> 通过`System.out.prinln()`输出是会默认调用toString方法，如果自定义类，可以重写。





### ☆面试题：String,StringBuilder,StringBufferr区别
- String声明的是不可变的对象，每次操作都会生成新的对象，然后将指针指向新的对象
- StringBuilder，StringBuffer可以在原有对象上修改，**StringBuider**为非线程安全,性能好




### String空判断

- `if(str == null || str.equals(""))`

- `if(str == null || str.length() != 0 )`

- `if(str == null || str.isEmpty())` SE6才开始使用

- `if(str == null || str == “”);`

  

### StringBuffer方法
- StringBuffer append(String s) 将指定的字符串追加到此字符序列
- StringBuffer reverse() 将此字符序列用其反转形式取代
- delete(int start, int end) 移除此序列的子字符串中的字符
- replace(int start, int end, String str) 使用给定 String 中的字符替换此序列的子字符串中的字符
- insert(int offset, int i) 将 int 参数的字符串表示形式插入此序列中

### String方法
- int length() 返回此字符串的长度
- char charAt(int index) 返回指定索引处的字符
- int indexOf(int ch) 返回指定字符在此字符串中第一次出现处的索引
- String substring(int beginIndex, int endIndex)返回一个新字符串，它是此字符串的一个子字符串
- boolean equals(Object anObject) 将此字符串与指定的对象比较
- boolean equalsIgnoreCase(String anotherString) 将此 String 与另一个 String 比较，不考虑大小写
- int compareTo(String anotherString) 按字典顺序比较两个字符串
- int compareToIgnoreCase(String str) 按字典顺序比较两个字符串，不考虑大小写
- String concat(String str) 将指定字符串连接到此字符串的结尾
- String replace(char oldChar, char newChar) 返回一个新的字符串，newChar替换此字符串中出现的所有oldChar
- String trim() 返回字符串的副本，忽略前导空白和尾部空白
- String[] split(String regex) 根据给定正则表达式的匹配拆分此字符串
- String toUpperCase()/String toLowerCase() 字符串大小写转换


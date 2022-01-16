---
title: java中的Scanner
date: 2020-03-15 21:09:38
tags: Java
categories: java基础
---
## Scanner类
它提供了输入数据的方法、包含在被称为“实用类”的**java.util**包中；
在使用前需要创建一个Scanner对象；
声明一个名为in的Scanner变量，并新建一个Scanner对象以便从System.in中获取输入；
🌂：Scanner in = new Scanner(System.in);

```java
package demo1;

import java.util.Scanner;

public class Test_1 {

	public static void main(String[] args) {
		String line;
		Scanner in = new Scanner(System.in);
		
		System.out.print("小黄说:");
		line = in.nextLine();
		System.out.println("系统检测到小黄说了："+line);
		
	}
}
```

在这之间我发现nextline与next都是接收String类型；那它们究竟有什么区别呢？
下面用代码测试

```java
package demo1;

import java.util.Scanner;

public class Test_1 {

	public static void main(String[] args) {
		String line;
		Scanner in = new Scanner(System.in);
		
		System.out.print("Type something(nextline)：");
		line = in.nextLine();
		System.out.println("You said："+line);
		
		System.out.print("Type something(next)：");
		line = in.next();		
		System.out.println("You said："+line);
	}
}
//比如两个都输入 wo ai ni
//很明显看出差别

```
### 解释：
next()方法读取到空白符就结束l；

nextLine()读取到回车结束也就是“\r”；




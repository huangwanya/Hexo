---
title: Java之数组遍历&反转&修改
date: 2020-03-16 21:01:00
tags: Java
categories: java基础
---
## 前言：
这个特别简单，是有同学私信，才写这个的

## 1、字符串的遍历
在Java中，字符串提供了提取字符的方法**charAt**，这个方法会返回一个char，因此我们可以利用这一点来实现字符串的遍历；
```java
package demo1;

public class Test_3 {
	public static void main(String[] args) {
		String str ="abcdefg";
		for(int i=0;i<str.length();i++) {
			char letter = str.charAt(i);
			System.out.println(letter);
		}
	}
}
```
首先呢通过这个方法对字符串进行遍历，得到一个新的，原理很简单
## 2、字符串的反转

```java
package demo1;

public class Test_3 {
	public static void main(String[] args) {
		String str ="abcdefg";
		String str2 = reverse(str);
		printArr(str);
		printArr(str2);
	}
	private static void printArr(String str) {
		for(int i=0;i<str.length();i++) {
			char letter = str.charAt(i);
			System.out.print(letter);
		}
		System.out.println("");
	}
	public static String reverse(String s) {
		String a ="";
		for (int i = s.length() - 1;i>=0;i--) {
			a= a + s.charAt(i);
		}
		return a;
	}
}
```
## 2、字符串的修改
用的方法就是使用replace，它可以在字符串中查找并替换指定的字串。例如下面的代码讲把“aaa”替换为“bbb”；
```java
package demo1;

public class Test {
	public static void main(String[] args) {
		String text = "aaa";
		text = text.replace("aaa", "bbb");
		System.out.println(text);
	}
}

```
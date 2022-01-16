---
title: java之常量与变量
date: 2020-02-07 15:30:51
tags: Java
categories: java基础
---
## 1.常量：
### 1.1声明常量
```java
package rt;

/**
 * 语法： final 数据类型 常量名称 = 值
 * 规范：常量名称通常使用大写字母，比如PI、YEAR等等
 * 规则：常量名称符合标识符的要求，只能用字母、数字、_、$组成，不能以数字开头、不能使用关键词
 */
public class Test{
    public static void main (String[] args) {
      final int YUAR=365;   //定义在main里面不需要加ststic，若定义在外面则需要加
        System.out.println("一年有："+YUAR+"天");
        System.out.println("两年有："+YUAR*2+"天");
    }
}
```
若把YUAR定义在main外面则需要加static
```java
//若把YUAR定义在外面则为：
public class Test{
   static final int YUAR=365;   //定义在main外面需要加ststic
    public static void main (String[] args) {
        System.out.println("一年有："+YUAR+"天");
        System.out.println("两年有："+YUAR*2+"天");
    }
}
```
## 2.变量

### 2.1声明变量
```java
package rt;

public class Test{
    public static void main (String[] args) {
        int a=10;
        System.out.println(a);
       //改变a的值
        a=11;
        System.out.println(a);
    }
}
```
### 2.2变量的作用范围

按作用范围分类：成员变量和局部变量

#### 1.成员变量
在类体中定义的变量，作用范围为整个类，这个类中都可以访问到定义的这个变量
```java
public class Test4_2 {
	static int k = 1;   //即为成员变量---在类体中定义
	public static void main(String[] args) {
	
	}

}
```
#### 2.局部变量
在一个函数（方法）或代码块中定义的变量
特点：局部变量在方法或代码块被执行的时候创建，在结束时被销毁
`下面给一个简单的例子`
```java
public class Test4_2 {

	public static void main(String[] args) {
		int a = 1;
		//以下就是一个块
		{
			int b = 2;
			System.out.println("a="+a);
			System.out.println("b="+b);
		}
		int b = 3; //因为在上面执行结束后 代码块就被销毁了
		System.out.println("a="+a);
		System.out.println("b+"+b);
	}

}

```
结果为：
```java
a=1
b=2
a=1
b+3
```
    成员变量可以与局部变量重名
    其调用服从“就近原则”
    下面给出一个例子
    如果不删除int var = 2，结果会显示 2，删除后则为1；
	
```java
public class Test4_2 {
	static int var = 1;
	public static void main(String[] args) {
		int var = 2;
		System.out.println("the value of var = "+var);
	}

}

例二：
public class Test4_2 {
	static int var = 1;
	public static void main(String[] args) {
		int var = 2;
		System.out.println("the value of var = "+var);
		pt();
	}
	public static void pt() {
		System.out.println("the value of var = "+var);
	}

}

```

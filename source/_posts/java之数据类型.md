---
title: java之数据类型
date: 2020-02-07 15:52:06
tags: Java
categories: java基础
---
# 一、各种数据类型所占空间（没必要去记忆，了解即可）
### ①基本
字 节------byte------1字节
短整型------short------2字节
整 型------ int ------4字节
长整型------ long------8字节
布尔型------boolean—1bit
单精度------ float ------4字节
双精度------double------8字节
字 符------ char ------2字节

### ②包装类
整数类型对应的包装类
为什么引入包装类？万物即对象，而像byte、short等不是对象，所以由包装类引入对象的概念；
通常在后面加一个.可以得到其属性；
详细见下面的代码
byte------Byte
short------Short
int------Integer
long-----Long
float-----Float
double-----Double
char------Character


```java
public static void main(String[] args) {
		byte byte_max = Byte.MAX_VALUE;   //最大值
		byte byte_min = Byte.MIN_VALUE;   //最小值
		System.out.println("the maximum of byte is "+byte_max);
		System.out.println("the minimum of byte is "+byte_min);
		System.out.println("byte对应的比特位"+Byte.SIZE); 
		System.out.println("byte对应的类型"+Byte.TYPE);
	}

```
    小补充
    在Java中数字默认是int类型；
    所以在初始化赋值long类型时候应该 long 变量 = 1L;（L小写也可）
    int num = 1; //定义一个int类型
    long num1 = 1;	//设计自动转换，因为在java中默认数字为int类型
    long num 2 = 2L;	//定义一个long类型
    char ch = 91; 代表ch为’a‘;
    当要查看 字符对应的ASCII码时
    可以int ch2 = 'a';
    同理可以查看中文的GBK码表，此编码表也兼容ASCII码；
    这里同时拓展一下Unicode：将所有国家的编码表融合到一个表中
    
## 🌂布尔类型（没啥好说的）
```java
boolean b1 = true;
		boolean b2 = false;
		System.out.println(b1);
		System.out.println(b2);
```
# 二、数据类型转换（也没啥好说的前面说过）
但注意区别下这个的结果```java
int a = 55,b = 9;
		float g,h;
		g=a/b;
		h=(float)a/b;
		System.out.println(g);   //6.0
		System.out.println(h);	 //6.111111


# 三、基本数据类型的默认值三、基本数据类型的默认值

`成员变量有默认值，而局部变量没有`
下面即为成员变量的默认值：
byte—(byte)0；
short—(short)0;
int—0;
long—0L;
float—0.0F;
double—0.0D;
char— ’ '或\u0000(也代表空，它为Unicode字符) 打印时像这样使用’\u0000‘
boolean—false;

# 四、拓展
🌂（布尔类型）在c\c++中规定所有非零数即为真，零为假；而在Java中，布尔变量只有true与false两个变量，除此之外没有其他的任何值，因而它和任何数字都无关；
### ①打印int类型中最小值到最大值是否为偶数
```java
public static void main(String[] args) {
		for(int i = Integer.MIN_VALUE;i<=Integer.MAX_VALUE;i++)
		{
			boolean isEven = (i%2==0);
			System.out.println("i = "+i+",isEven = "+isEven);
也等价于		System.out.println("i=%d,isEven=%b",i,isEven);
		}

```
### ②拼接符号的使用
🌂“+”的功能:加法运算与拼接符号
加法运算：只有数字的时候，结果得到数字；
拼接符号：有字符串的时候，与字符串进行相加，得到字符串；
```java
 public static void main(String[] args) {
        int x1 = 5;
        int x2 = 2;
        System.out.println(x1+x2);      //7
        System.out.println(x1+x2+"1");  //71
        System.out.println(x1+x2+"K");  //7K
        System.out.println("A"+x1+x2);  //A52
    }
```
解释一下最后一个：
“A”+x1为字符串与数字相加得到“A5”这个字符串再与x2相加得到字符串“A52"；


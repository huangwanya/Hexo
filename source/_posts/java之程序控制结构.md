---
title: java之程序控制结构
date: 2020-03-09 20:43:59
tags: Java
categories: java基础
---
## 一、选择结构(了解下就好)

### 1.1条件分支（if与if-else语句）

```java
那就先说说if语句，其实没啥好讲的if（条件为真）执行，否则不执行！！！
记住就行，没啥太过于纠结和不懂的地方！！
package demo1;

public class Test {

	public static void main(String[] args) {
		int num = 1;
		if(num==1) {
			System.out.println("Right1");
		}
		if(num==2) {
			System.out.println("Right2");
		}
	}
}
程序运行结果为Right1


```
### 1.2 if-else语句	


    基本部分： if（true）
    {
    执行这里
    } else
    {
    执行这里（条件为假）
    }
```java
public static void main(String[] args) {
		int num = 1;
		if(num==1) {
			System.out.println("1");
		}else {
			System.out.println("2");
		}
	}

```

很通俗，下一个！！！

### 1.3多条件的if-else


    
    if(判断条件1)
    {
    -------语句块1；
    }else if(判断条件2)
    {
    --------语句块2
    }else
    {
    --------语句块3
    }
    这里不再过多叙述！！！
    
1.4多重选择—switch语句

`

    switch(表达式)
    {
    case 值1:语句块1;break;
    case 值2:语句块2;break;
    …
    default:语句块n；break;
    }`

其中其表达式类型为整型（long除外），字符型，枚举类型


## 二、循环结构（了解下就好）
### 2.1 while循环（当型循环）

    while(判断条件)
    {
    ------代码块
    }
	
	
	
	

### 2.2 do-while循环（不多说了）
### 2.3 for循环（也不想学，和c语言里面一模一样的结构）
### 2.4 foreach循环：用来循环遍历一个数组或集合框架
for（类型 迭代类型:数组或集合）{
					代码块
	}
毕竟我没接触过这个举个例子方便我以后回来复习看看


    实现数组的遍历，有一说一还挺方便哈哈哈
    public static void main(String[] args) {
    		int arr[] = {1,2,3,4,5};
    		for(int item:arr) {
    			System.out.println(item);
    		}
    }
    
### 2.5 嵌套循环（不多说表面意思）
## 三、循环的跳转
**break;continue;return;**
这里注意一下这三个的关系就行，作用自己敲一遍就理解了

### 3.1 break ：跳出所在的循环（switch、for、while）
```java
public static void main(String[] args) {
		int i;
		for(i=1;i<10;i++)
		{
			if(i%3==0) break;
			System.out.println(i);
		}
	}


```
### 3.2 contiue:结束本次循环进入下一次循环
```java
public static void main(String[] args) {
		int i;
		for(i=1;i<11;i++)
		{
			if(i%3==0) continue;
			System.out.println(i);	
		}
	}

```
### 3.3 return:离开语句所在的方法
```java
public static void main(String[] args) {
		int i;
		for(i=1;i<11;i++)
		{
			if(i%3==0) return;
			System.out.println(i);	
		}
	}

```


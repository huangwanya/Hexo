---
title: Java之方法
date: 2020-03-14 19:03:56
tags: Java
categories: java基础
---
## 1、方法的基本定义
修饰符 返回值类型 方法名（参数列表）{
方法体
[return 返回值]
}
🌂比如下面这个例子：

```java
package demo1;

public class T1 {
	public static int max(int num1,int num2) {
		int result;
		if(num1>num2) {
			result = num1;
		}else {
			result = num2;
		}
		return result;
	}
}

```

## 2、方法的使用
Add:this代表本类对象
哪个对象调用这个方法this就是它；
比如Person p1 = new Person(); 此时this代表p1；
```java

Person.java

package demo2;

public class Person {
	String name;
	int age;
	void talk() {
		System.out.println("冲冲冲！！！");
		System.out.println("我是"+name+"今年"+age+"岁");
	}
	//当局部变量与成员变量重名的时候用this区分 
	void setName(String name){
		this.name = name;//将局部变量赋值给成员变量
	}
	void setAge(int age) {
		this.age = age;
	}
}

```

```java
Personbtest.java

package demo2;

public class PersonbTest {

	public static void main(String[] args) {
		Person p1 = new Person();
		p1.setName("hw");    //调用方法
		p1.setAge(21);
		p1.talk();
	}

}
```

## 3、方法中的形参与实参
**形参**：隶属于方法体，是方法的局部变量
在调用方法时，实参和形参在数量、类型、顺序上必须保持一致；

## 4、方法的重载
**三要素：**必须在同一个类；必须方法名相同；必须参数列表不同（个数与类型）
注意：重载定义与返回值无关
🌂比如：

```java
package demo1;

public class T2 {
	public int add(int a,int b) {
		return a+b;
	}
	public int add(int a,int b,int c) {
		return a+b+c;
	}
	public float add(float a,float b) {
		return a+b;
	}
}

```
## 5、构造方法
构造方法就算再每一个类中定义的，并且是再使用new关键字实例化一个新对象的时候默认调用的方法。
构造方法的功能是对新创建对象的成员变量赋值
语法结构：访问修饰符 类名（[参数列表]）{
功能代码
}

**注意：**
1、构造方法的名次必须与所属类类名一致
2、构造方法无返回值，也不可以使用void
3、构造方法可以被重载
4、构造方法不能被static和final修饰
5、构造方法不能被继承，子类使用父类的构造方法需要使用super关键字
规定：如果某一个类中，没有显式的定义一个构造方法，那系统会默认给其一个无参的构造方法，并且方法体式空的构造方法；

```java
package demo3;

public class Person {
	int age;
	String name;
	public Person(int age) {
		this.age = age;
		System.out.println("构造方法Person(int x)被调用");
		System.out.println("age="+this.age);
	}
	public Person() {
		System.out.println("构造方法Person()被调用");
	}
	public Person(String name,int age) {
		System.out.println("构造方法Person(String name,int age)被调用");
		this.age = age;
		this.name = name;
	}
	public void talk() {
		System.out.println("name="+name+"age="+age);
	}
}

```

构造方法的私有化：仅仅在当前类可以使用！！！主要用于完成单例模式；

## 6、在方法中调用方法
1、 如果调用本类的方法,直接使用方法名（[实际]）
2、如果调用的是其他类的方法，还是需要创建对象，然后通过对象名.方法（[实参]）的形式调用 ；
3、如果调用的方法是静态的，可以通过类名.方法名（[实参]）或接口名.方法名([实参]);
🌂快捷键：Alt+Shift+s 封装Getters和Setters；
```java

package demo4;

public class Person {
	private String name;
	private int age;
	
	private void talk() {
		System.out.println("我是"+name+",今年："+age+"岁");
	}
	private void say() {
		talk();
		//this.talk();
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	
}

```

## 7、方法的递归调用

```java
package demo5;

public class Test {
	public int add(int n){
		int result = 0;
		for(int i=1;i<n+1;i++) {
			result += i;
		}
		return result;
	}
	
	public int addRecursion(int n) {
		if(n==1) {
			return n;
		}else {
			return n+addRecursion(n-1);
		}
	}
	
}

```

## 8、代码块
格式：{ 语句}
分为四种；
1、普通代码块；
2、构造代码块；
3、静态代码块；
4、同步代码块；
//普通代码块：方法名后或方法体内用一对“{}”括起来的数据库；
```java

package demo6;

public class NormalCodeBlock {
	public static void main(String[] args) {
		{
			int x = 10;
			System.out.println("普通代码块内，x="+x);
		}
		int x = 100;
		System.out.println("x="+x);
	}
}

```

//构造代码块
```java

package demo6;

public class GzCodeBlock2 {
	private String name;
	private int x;
	//构造代码块  
	{
		System.out.println("构造代码块");
		x= 100;
		show();
	}
	//无参构造方法
	GzCodeBlock2(){
		System.out.println("构造");
		name = "张三";
	}
	//有参构造方法
	GzCodeBlock2(String name){
		System.out.println("hh");
		this.name = name;
		show();
	}
	void show() {
		System.out.println("welcome"+name);
		System.out.println("x="+x);
	}
}

```

//静态代码块
加一个static即可；执行一次；

//同步代码块
以后再写

## 9、方法和数组

```java
package demo4;

public class Test_1 {
	public static void main(String[] args) {
		int in = 10;
		int[] arr = new int[]{1,2,3,4,5};
		System.out.println("---调用changeReferVAlue前---");
		printArr(arr);
		changeReferValue(in, arr);
		System.out.println("---调用changeReferVAlue后---");
		print(in, arr);
	}
	public static void changeReferValue(int a,int[] myArr) {
		a+=1;
		for(int i=0;i<3;i++) {
			myArr[i] = 0;
		}
	}
	public static void printArr(int[] arr) {
		for(int i:arr) {
			System.out.print(i+"\t");
		}
		System.out.println("");
	}
	public static void print(int in,int[] arr) {
		System.out.println("in:"+in);
		System.out.println("arr:");
		printArr(arr);
	}

}
```



返回数组的方式：
```java
package demo4;

public class ArrayReturn {
	public static int[] sort(int[] arr) {
		for(int i = 0;i<arr.length-1;i++) {
			for (int j = 0; j < arr.length-i-1; j++) {
				if(arr[j]>arr[j+1]) {
					int temp =arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;
				}
			}
		}
		return arr;
	}
	public static void printArr(int[] arr,String msg) {
		System.out.println(msg);
		for(int i:arr) {
			System.out.print(i+"\t");
		}
		System.out.println("");
	}
	public static void main(String[] args) {
		int[] arr = {3,5,2,6,8,4,7,9};
		int arrNew[];
		printArr(arr,"排序前");
		arrNew = sort(arr);
		printArr(arrNew, "排序后");
	}
}

```

## 10、与数组有关的操作方法
### 1、数组的克隆
克隆对象返回的是一个新的对象，而不是已有对象的引用
克隆对象与new操作符是有区别的，克隆对象是拷贝某个对象的当前信息，而不是对象的初始化信息；

```java
package demo4;

public class ArrraytMethod {
	public static void printArr(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i]+"\t");
		}
		System.out.println("");
	}
	//克隆内容相同，地址不同
	public static void main(String[] args) {
		int[] arr= {3,5,7,3,7,89,54,3,2,65,7};
		printArr(arr);
		System.out.println(arr);
		int[] arrNew = arr.clone();
		printArr(arrNew);
		System.out.println(arrNew);
	}
}

```

### 2、利用系统类库排序

```java
package demo4;

import java.util.Arrays;

public class ArrraytMethod2 {
	public static void printArr(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i]+"\t");
		}
		System.out.println("");
	}
	//使用Arrays的sort方法，默认从小到大排序
	public static void main(String[] args) {
		int[] arr= {3,5,7,3,7,89,54,3,2,65,7};
		System.out.println("排序前");
		printArr(arr);
		Arrays.sort(arr);
		System.out.println("排序后");
		printArr(arr);
	}
}

```


## 11、除去一个数组中的0

```java
package demo7;

public class EX10_1 {
	public static void main(String[] args) {
		int[] oldArr = {1,3,4,5,0,0,6,6,0,5,4,7,6,7,0,5};
		int count = getValueNumFromArray(oldArr, 0);
		int[] newArr = new int[oldArr.length-count];
		copyValue(oldArr, newArr);
		printArray(newArr);
	}
	public static int getValueNumFromArray(int[] arr,int val) {
		int count = 0;
		for (int i = 0; i < arr.length; i++) {
			if(arr[i]==val) {
				count++;
			}
		}
		return count;
	}
	public static void copyValue(int[] arr1,int[] arr2) {
		int j = 0;
		for (int i = 0; i < arr1.length; i++) {
			if(arr1[i]!=0) {
				arr2[j] = arr1[i];
				j++;
			}
		}
	}
	
	public static void printArray(int[] array){
		for (int i = 0; i < array.length; i++) {
			System.out.println(array[i]+"\t");
		}
		System.out.println("");
	}
	
}
```

## 12.两个数之间的随机数

```java
package demo7;

public class EX10_2 {
	public static void main(String[] args) {
		int num1 = 2;
		int num2 = 17;
		for(int i=0;i<100;i++) {
			System.out.println(getRandom(num1, num2));
		}
	}
	public static int getRandom(int num1,int num2) {
		int result = (int)Math.random()*(num2-num1)+num1;
		return result;
	}
}
```


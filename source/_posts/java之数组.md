---
title: java之数组
date: 2020-03-12 10:52:55
tags: Java
categories: java基础

---
## 前言：
这里是复习没啥好说的，随便复习一下概念和写几个实例，一下子就懂了！

## 一、一维数组
**定义：**类型[ ] 数组名 = new 类型[长度]；
数组中存有默认值0，而在引用类型[ ]中为null;

如果在定义前，已经知道数组里存放的内容，那可以简单定义为：
类型[ ] 数组名 = {值1，值2，…，值n}；
类型[ ] 数组名 = new 类型[ ]{值1，值2，…，值n}；

Add:
动态初始化如：int[ ] array = new int[4];
静态初始化如：int[] array = new int[]{1,2,3,4};
但是像int[] array = new int[3]{1，2，3};就是错误的写法
这样也是错的，int[ ] array；array[ ] = {1,2,3};也是错的！！！

**而这样是可以的：把String names[ ] = new String[ ]{“加油”，“冲呀”}；
拆为：String names[ ] ；和 names[ ] = new String[ ]{“加油”，“冲呀”}；**

这里老师拓展了一个知识点：就是创建一个随机的数组

```java

package demo;
import java.util.Random;    //导入包

public class Tst_1 {
	public static void main(String[] args) {
		Random rand = new Random();
		int[] a = null;
		a = new int[rand.nextInt(10)]; //开辟内存空间，长度是[0，10）的随机数
		System.out.println("数组的长度："+a.length);
		for(int i=0;i<a.length;i++) {
			a[i] = rand.nextInt(100);
			System.out.println("a["+i+"]="+a[i]);
		}
	}
}

```

🌂另外，Java按引用传值的方式也有点不太一样！！！
Add:两个数组对应同一个内存区域！！！

```java
package demo;


public class Tst_1 {
	public static void main(String[] args) {
			int[] nums1 = {1,2};
			int[] nums2 = nums1;  //这里其实把nums1中记录的数组的地址传给了nums2
			//现在遍历两个数组
			System.out.println("第一个数组");
			for(int x:nums1) {
				System.out.println(x);
			}
			System.out.println("第二个数组");
			for(int y:nums2) {
				System.out.println(y);
			}
			//现在 修改下第二个数组中元素值
			nums2[1] = 100;
			System.out.println("第一个数组");
			for(int x:nums1) {
				System.out.println(x);
			}
			System.out.println("第二个数组");
			for(int y:nums2) {
				System.out.println(y);
			}
	}
}
输出结果为：
第一个数组
1
2
第二个数组
1
2
第一个数组
1
100
第二个数组
1
100

```

```

这里再来一个简单的比较最大最小值的代码！！！
```java

package demo;


public class Tst_1 {
	public static void main(String[] args) {
			int nums[] = {100,5,41,79,58,1000,9854,121};
			int max = nums[0];
			int min = nums[0];
			for(int i=1;i<nums.length;i++) {
				if(nums[i]>max) {
					max = nums[i];
				}
			}
			for(int i=1;i<nums.length;i++) {
				if(nums[i]<min) {
					min = nums[i];
				}
			}
			System.out.printf("该数组中最大值为：%d，最小值为：%d",max,min);
	}
}
```

## 二、二维数组
定义：数据类型[ ][ ] 数组名 = new int[行数][列数];
如：int[ ][ ] array = new int [3][2];或者int[ ][ ] array; array = new int[3][2];
实质我感觉就像是数组的数组
下面举一个行数列数都定了的二维数组
```java

package demo;

public class Tst_2 {

	public static void main(String[] args) {
		int[][] nums;
		nums = new int[3][2];
		for(int i=0;i<nums.length;i++) {
			for(int j=0;j<nums[i].length;j++) {
				System.out.println("nums["+i+"]["+j+"]="+nums[i][j]);
			}
		}
	}

}

```
再举例列数不定（一定记得用new初始化）

```java
package demo;

public class Tst_2 {

	public static void main(String[] args) {
		int[][] nums = new int[3][];
		nums[0] = new int[2];
		nums[1] = new int[2];
		nums[2] = new int[] {1,2};
		for(int i=0;i<nums.length;i++) {
			for(int j=0;j<nums[i].length;j++) {
				System.out.println("nums["+i+"]["+j+"]="+nums[i][j]);
			}
		}
	}

}
```


🌂如果知道数组中存的值还可以：

```java
package demo;

public class Tst_2 {

	public static void main(String[] args) {
		int[][] nums = {{1,2},{1,2,3},{2}};
		for(int i=0;i<nums.length;i++) {
			for(int j=0;j<nums[i].length;j++) {
				System.out.println("nums["+i+"]["+j+"]="+nums[i][j]);
			}
		}
	}

}

```

下面补充一道例题，计算某个员工的工资总额

```java
package demo;

public class Tst_2 {

	public static void main(String[] args) {
		int sum = 0;
		int[][] nums = {{1,2},{1,2,3},{2}};
		for(int i=0;i<nums.length;i++) {
			System.out.printf("第%d个人的销售总额为：",(i+1));
			for(int j=0;j<nums[i].length;j++) {
				sum += nums[i][j];
			}
			System.out.println(sum);
			sum = 0;   //清零便于下一次计算
		}
	}

}

```

## 三、多维数组
定义：数据类型[ ][ ]…[] 数组名 = new int[ ][ ]…[];
类似的写法，没啥可说的！！！
## 四、拓展
### 4.1 Java中null的使用：可以为引用类型赋值，作为初始值4.1 Java中null的使用：可以为引用类型赋值，作为初始值
比如：
```java

public static void main(String[] args) {
		String name = null;
		name = "冲冲冲";
		System.out.println(name);
		
	}

```
🌂如果想要比较两个字符串：可以使用a.equals(b)或b.equals(a);返回一个布尔类型；
🌂另外如果报错信息出现NullPointerException则考虑是空对象；

### 4.2冒泡排序（参考曾经学c的思路）
1
```java
package demo;

public class Tst_3 {

	public static void main(String[] args) {
		int[] a = {900, 2, 3, -58, 34, 76, 32, 43, 56, -70, 35, -234, 532, 543, 2500};
	    int n;  //存放数组a中元素的个数
	    int buf;  //交换数据时用于存放中间数据
		for(int i=0;i<a.length-1 ;i++) {
			for(int j=0;j<a.length-i-1;j++) {
				if(a[j]<a[j+1]) {
					buf = a[j];
					a[j] = a[j+1];
					a[j+1] = buf;
				}
			}
		}
		for(int x:a) {
			System.out.println(x);
		}
	}

}

```

### ·4.3复习下随机数（乱序）

```java
package demo;
import java.util.Random;
public class Tst_4 {

	public static void main(String[] args) {
		Random random = new Random();
		int buf;
		int[] nums = new int[random.nextInt(10)];
		for(int i=0;i<nums.length;i++) {
			nums[i] = random.nextInt(100);	
		}
		for(int i=0;i<nums.length-1 ;i++) {
			for(int j=0;j<nums.length-i-1;j++) {
				if(random.nextBoolean()) {
					buf = nums[j];
					nums[j] = nums[j+1];
					nums[j+1] = buf;
				}
			}
		}
		for(int x:nums) {
			System.out.println(x);
		}
}

}
```


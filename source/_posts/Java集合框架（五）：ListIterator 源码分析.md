---
title: Java集合框架（五）：ListIterator 源码分析
date: 2020-03-31 17:59:19
tags: 框架
categories: java集合框架
---
## 1、ListIterator 接口简述

在Java中，ListIterator 是Collection API中的一个接口。 它扩展了Iterator接口。它是一个双向迭代器。 为了支持前向和后向迭代和CRUD操作，它具有以下方法。 我们可以将这个Iterator用于所有List实现的类，如ArrayList，CopyOnWriteArrayList，LinkedList，Stack，Vector等。

## 2、ListIterator 接口类图
![](https://s1.ax1x.com/2020/03/31/GKdmvQ.png)
## 3、ListIterator 接口方法摘要
Java ListIterator 接口包含以下几个方法：

方法  |      描述
-------------- | -------------
void add(E e)  | 将指定的元素插入列表中
boolean hasNext()  |如果此列表迭代器在向前遍历列表时还有更多元素，则返回true
boolean hasPrevious()|如果此列表迭代器在反向遍历列表时还有更多元素，则返回true
E next()|返回列表中的下一个元素并前移光标位置
int nextIndex()|返回下一个元素的索引
E previous()|返回列表中的上一个元素并向后移动光标位置。
int previousIndex()|返回后续调用previous（）返回的元素的索引。
void remove()|从列表中删除next（）或previous（）的最后一个元素。
void set(E e)|用指定的元素替换next（）或previous（）返回的最后一个元素

## 4、ListIterator 应用基本示例
### 4.1、如何获得ListIterator？
```java
ListIterator<E> listIterator()
它返回此列表中元素的列表迭代器。

```
```java

import java.util.*;

public class ListIteratorDemo 
{
  public static void main(String[] args) {
        List<String> names = new LinkedList<>();
        names.add("Apple");
        names.add("Orange");
        names.add("Banana");

        // 获取 ListIterator 对象
        ListIterator<String> listIterator = names.listIterator();

        // 遍历元素
        System.out.println("ListIterator遍历:");
        while(listIterator.hasNext()){
            System.out.println(listIterator.next());
        }
        System.out.println("for循环遍历:");
        for(String name: names){
            System.out.println(name);
        }
    }
}


```
**输出**
```java
ListIterator遍历:
Apple
Orange
Banana

for循环遍历:
Apple
Orange
Banana

```
### 4.2、ListIterator双向迭代示例
```java

import java.util.*;

public class BiDirectinalListIteratorDemo 
{
	 public static void main(String[] args) {
        List<String> names = new LinkedList<>();
        names.add("Apple");
        names.add("Orange");
        names.add("Banana");

        // 获取 ListIterator 对象
        ListIterator<String> listIterator = names.listIterator();

        // 正向遍历元素
        System.out.println("正向遍历:");
        while(listIterator.hasNext()){
            System.out.println(listIterator.next());
        }

        // 反向遍历元素
        System.out.println("反向遍历元素:");
        while(listIterator.hasPrevious()){
            System.out.println(listIterator.previous());
        }
    }
}


```
**输出**
```java
正向遍历:
Apple
Orange
Banana
反向遍历元素:
Banana
Orange
Apple

```

## 5、Java迭代器的类型
我们知道Java有四个游标：Enumeration，Iterator，ListIterator和Spliterator。 我们将它们分为两种主要类型，如下所示：

🌂单向迭代器
仅支持正向迭代。 例如，Enumeration，Iterator等都是单项迭代器

🌂双向迭代器
支持正向和反向迭代。 例如，ListIterator 就 是双向迭代器。

## 6、Java ListIterator 内部是如何工作的？
由上文我们知道，Java ListIterator 是一个双向迭代器，为了支持此功能，它有两组方法以实现该功能。

1、正向迭代方法
🌂hasNext())
🌂next()
🌂nextIndex()

2、反向迭代方法
🌂hasPrevious()
🌂previous()
🌂previousIndex()

### 6.1、正向迭代实现分析
正向迭代实现的分析可参见前一篇文章：Java集合框架（四）：Iterator 源码分析
![](https://s1.ax1x.com/2020/03/31/GKdZ8S.png)

### 6.2、反向迭代实现分析
![](https://s1.ax1x.com/2020/03/31/GKdKDs.png)
```java
List<String> names = new LinkedList<>();
names.add("E-1");
names.add("E-2");
names.add("E-3");
................
names.add("E-n");

```
使用 LinkedList 创建一个ListIterator对象，如下所示：

```java
ListIterator<String> namesIterator = names.listLterator();
```
我们假设当前 namesIterator 迭代器如下所示：
![](https://s1.ax1x.com/2020/03/31/GKdA4f.png)



这里 ListIterator 的Cursor指向List的第一个元素的前边。 现在我们运行以下代码：
 ```java
 namesIterator.hasPrevious();
  namesIterator.previous();
```
![](https://s1.ax1x.com/2020/03/31/GKdkUP.png)
当我们运行上面的代码时，ListIterator的Cursor指向LinkedList中的最后一个元素，如上图所示。 接着，我们运行以下代码：
```java

  namesIterator.hasPrevious();
  namesIterator.previous();
```
![](https://s1.ax1x.com/2020/03/31/GKdlEq.png)
当我们运行上面的代码时，ListIterator 的 Cursor 指向List中最后一个元素的前一个元素，如上图所示。 循环此过程以将ListIterator的Cursor传递到LinkedList的第一个元素。
![](https://s1.ax1x.com/2020/03/31/GKdFEt.png)
在读取第一个元素后，如果我们继续执行下面的代码

```java
namesIterator.hasPrevious();
```
它将返回 false，以终止循环，至此反向遍历完成。
![](https://s1.ax1x.com/2020/03/31/GKdVC8.png)
## 7、Java ListIterator 局限性

与Iterator相比，Java ListIterator有许多优点。 但是，它仍然存在以下一些限制。

🌂它是Iterator唯一的List实现类。

🌂与Iterator不同，它不适用于整个Collection API。

🌂与Spliterator相比，它不支持元素的并行迭代。

🌂与Spliterator相比，它不支持更好的性能来迭代大量数据。

## 8、ListIterator 与 Iterator 的区别

Iterator|ListIterator
-------------- | -------------
在Java 1.2中引入|在Java 1.2中引入
它是整个Collection API的迭代器|它仅用于List 及其实现类
它仅支持单项迭代|它是双向迭代器，支持正反向迭代
它仅支持READ 和 DELETE 操作|它支持CRUD操作
可以使用iterator（）方法获取Iterator|可以使用listIterator（）方法获取ListIterator对象
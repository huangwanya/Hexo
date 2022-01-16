---
title: Java集合框架（七）：SortedSet 源码分析
date: 2020-04-03 20:20:25
tags: 框架
categories: java集合框架
---

## 1、SortedSet 简述

SortedSet 接口扩展了 Set 接口并提供了元素的排序功能。

插入到**有序集**中的所有元素必须实现**Comparable**接口（或者被指定的Comparator接受），并且所有这些元素必须是可相互比较的，比如：**e1.compareTo(e2)**（或 comparator.compare(e1, e2)）对于有序集合中的任意元素 e1 和 e2 都不能抛出 ClassCastException。
试图违反规则的方法或者构造方法，调用时会抛出 ClassCastException。

如果有序集合**正确实现了 Set 接口**，则有序集合所保持的顺序（无论是否明确提供了比较器）都必须保持相等一致性（相等一致性的定义请参阅 Comparable 接口或 Comparator 接口）。

这也是因为** Set 接口是按照 equals 操作定义的**，但有序集合使用它的** compareTo** 或** compare** 方法对所有元素进行比较， 因此从有序集合的角度来看，此方法认为相等的两个元素就是相等的。
即使顺序没有保持相等一致性，有序集合的行为仍然是良好的，只不过没有遵守 Set 接口的常规协定。

**所有通用有序集合实现类都应该提供四个“标准”构造方法：**
🌂一个void（无参数）构造函数，它根据元素的自然顺序创建一个空的有序集。

🌂具有Comparator类型的单个参数的构造函数，它创建一个根据指定的比较器排序的空的有序集。

🌂具有Collection类型的单个参数的构造函数，它创建一个具有与其参数相同元素的新排序集，并根据元素的自然顺序进行排序。

🌂具有SortedSet类型的单个参数的构造函数，它创建一个新的有序集，其具有与输入有序集相同的元素和相同的顺序。 由于接口不能包含构造函数，因此无法强制执行此建议。


除了 JDK 中实现类（ConcurrentSkipListSet,TreeSet）遵循此建议外，无法保证强制实施此建议（因为接口不能包含构造方法）。

`注意：有几种方法返回具有受限范围的子集。`
这样的范围是半开放的，即返回的子集元素是根据指定的范围（lowEndpoint 至 highEndpoint）从原始集合中截取的，这个范围，包括 lowEndpoint 但不包括 highEndpoint。

例如，假设 s 是一组有序的字符串。 以下语句获得 s 的一个子集，其中包含 s 中从 low 到 high 的所有字符串，包括 high：
```java
SortedSet<String> sub = s.subSet(low, high+"\0");
```
其中包含 s 中从 low 到 high 的所有字符串，不包括 high：
```java
SortedSet<String> sub = s.subSet(low+"\0", high);
```
## 2、SortedSet 类图

SortedSet 接口继承于 Set 接口，同时又提供了一些功能增强的方法，比如 comparator 从而实现了元素的有序性。
![](https://s1.ax1x.com/2020/04/03/Gag8L8.png)

🌂SortedSet 父接口
Collection, Iterable, Set

🌂SortedSet 子接口
NavigableSet

## 3、SortedSet 方法

方法  | 方法描述
------------- | -------------
comparator（）|返回用于对此集合中的元素进行排序的比较器，如果此集合使用其元素的自然顺序，则返回null。
first（）|返回此集合中当前的第一个（最低）元素。
headSet（E toElement）|返回此set的部分视图，其元素严格小于toElement。
last（）|返回此集合中当前的最后一个（最高）元素。
subSet（E fromElement，E toElement）|返回此set的部分视图，其元素范围从 fromElement（包括） 到 toElement（不包括）。
tailSet（E fromElement）| 返回此set的部分元素，其元素大于或等于fromElement。
spliterator()|在此有序集中的元素上创建Spliterator。

🌂从 java.util.Set 继承的方法有：
add, addAll, clear, contains, containsAll, equals, hashCode, isEmpty, iterator, remove, removeAll, retainAll, size, toArray, toArray

🌂从 java.util.Collection 继承的方法有：
parallelStream, removeIf, stream

🌂从 java.lang.Iterable 继承的方法有：
forEach

## 4、SortedSet 方法示例

```java
public static void main (String[] args) {
        SortedSet<String> a=new TreeSet<>();
        a.add("huangwan");
        a.add("lizi");
        a.add("liwenwen");
        a.add("xiangai");
        System.out.println("sortedSet:"+a);
        System.out.println("sortedSet First:"+a.first());
        System.out.println("sortedSet LAst:"+a.last());

        //获取 liwenwen(不包含)之前的元素
        SortedSet<String> b=a.headSet("liwenwen");
        System.out.println("获取 liwenwen(不包含)之前的元素:"+b);

        //获取包含lizi(包含)和xiangai(不包含)之间的元素
        SortedSet<String> c=a.subSet("lizi","xiangai");
        System.out.println("获取包含lizi(包含)和xiangai(不包含)之间的元素:"+c);

        //获取 liwenwen(包含)之后的元素
        SortedSet<String> d=a.tailSet("liwenwen");
        System.out.println("获取 liwenwen(包含)之后的元素:"+d);
    }
```
**输出结果：**
```java
sortedSet:[huangwan, liwenwen, lizi, xiangai]
sortedSet First:huangwan
sortedSet LAst:xiangai
获取 liwenwen(不包含)之前的元素:[huangwan]
获取包含lizi(包含)和xiangai(不包含)之间的元素:[lizi]
获取 liwenwen(包含)之后的元素:[liwenwen, lizi, xiangai]

```


## 5、注意事项

1、SortedSet 是“根据对象的比较顺序”，而不是“插入顺序”进行排序.

2、SortedSet 的实现不是同步的，不是线程安全的

如果多个线程同时访问一个 set，而其中至少一个线程修改了该 set，那么它必须保持外部同步.通常通过对某个自然类封装该 set 的对象进行同步来实现此操作。如果不存在此类对象，
则 set 就应该使用 Collections.synchronizedSet 方法进行“包装”。

此操作最好在创建时声明，以防止对 set 的意外非同步访问：

```java
 SortedSet s = Collections.synchronizedSortedSet(new TreeSet(...));
```
3、SortedSet 允许 null 元素.

4、SortedSet 的实现类 TreeSet 将使用其 compareTo（或 compare）方法执行所有的键比较，认为两个对象的键相等就表示它们两个对象是相等的。它违背了 Set 接口的常规协定。

5、SortedSet 的 iterator 方法返回的迭代器是快速失败的

在创建迭代器之后，如果对集合进行修改， 除非通过迭代器自身的 remove 方法，否则在任何时间以任何方式对其进行修改， Iterator 都将抛出 ConcurrentModificationException

6、iterator() 返回的迭代器，里面的元素是以升序排序的.

7、当试图添加一个重复元素到TreeSet时，新元素并不会把旧元素替换掉，
而只是新元素不会添加到TreeSet（不会抛异常。）

8、 TreeSet 时用红黑树的数据结构来为元素排序
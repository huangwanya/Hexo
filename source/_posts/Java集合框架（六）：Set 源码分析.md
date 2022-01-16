---
title: Java集合框架（六）：Set 源码分析
date: 2020-04-03 16:32:04
tags: 框架
categories: java集合框架
---
## 1、Set 简述

Java Set 是一个不包含重复元素的集合。它继承于 Collection 接口。 它有以下特点：

🌂与List不同，Java Set不是有序集合，它的元素没有特定的顺序。
🌂Java Set不提供对插入元素的位置的控制。
🌂它不能通过索引访问元素，但是可以搜索列表中的元素。
🌂Set允许最多仅添加一个null元素。
🌂Set接口在Java 8中有一个默认方法：spliterator。
## 2、Set 类图
Java Set 接口继承了 Collection 接口。 Collection 接口继承了 Iterable 接口。 一些常用的 Set 实现类有 HashSet，LinkedHashSet，TreeSet，CopyOnWriteArraySet 和 ConcurrentSkipListSet。AbstractSet 提供了 Set 接口的基本实现，以减少 Set 实现类的工作量。
![](https://s1.ax1x.com/2020/04/03/GUNiq0.jpg)

🌂Set 父接口：
```java
Collection<E>, Iterable<E>
```
🌂Set 子接口：
```java
NavigableSet<E>, SortedSet<E>
```
🌂Set 实现类：
```java
AbstractSet, ConcurrentHashMap.KeySetView, ConcurrentSkipListSet, CopyOnWriteArraySet, EnumSet, HashSet, JobStateReasons, LinkedHashSet, TreeSet
```
## 3、Set 方法

方法  | 方法描述
------------- | -------------
boolean	add(E e)|如果指定的元素不存在，则将其添加到此集合。如果此set已包含该元素，则保持set不变并返回false。
boolean addAll(Collection<? extends E> c)|如果指定集合中的所有元素不存在当前集合中，则将其添加到此集合中。 否则，addAll操作会修改此集合，使其值为两个集合的并集。 如果操作正在进行时修改了指定的集合，则此操作的行为是不确定的。
void clear()|清空当前结合中的所有元素
boolean contains(Object o)|如果此set包含指定的元素，则返回true。当且仅当此集合包含元素e时才返回true（o == null？e == null：o.equals（e））。
boolean containsAll(Collection<?> c)|如果此set包含指定collection的所有元素，则返回true。 如果指定的集合是当前集合的子集，则返回true。
boolean	equals(Object o)|将指定对象与此set进行相等性比较。 如果指定的对象也是一个集合，两个集合具有相同的大小，并且指定集合的每个成员都包含在此集合中，则返回true。
int	hashCode()|返回此set的哈希码值。 集合的哈希码被定义为集合中元素的哈希码的总和，其中空元素的哈希码被定义为零。 这意味着对于任何两个集合s1和s2的 s1.hashCode（）== s2.hashCode（）时，s1.equals（s2）。
boolean	isEmpty()|如果此set不包含任何元素，则返回true。
Iterator|iterator()	返回此set中元素的迭代器。 元素以无特定顺序返回
boolean	remove(Object o)|从该集合中移除指定的元素，如果该元素存在集合中，则返回true
boolean removeAll(Collection<?> c)|从当前集合中删除所有包含指定集合的元素
boolean retainAll(Collection<?> c)|仅保留当前集合中包含指定集合中的元素。 换句话说，该方法最终结果为两个集合的交集。
int	size()|返回此集合中的元素数。 如果此set包含 Integer.MAX_VALUE 个元素，则返回 Integer.MAX_VALUE。
default Spliterator spliterator()|在此集合中的元素上创建Spliterator
Object[] toArray()|返回包含此set中所有元素的数组。 返回的数组将是“安全的”，因为该集合不维护对它的引用。 （换句话说，即使此数组由数组支持，此方法也必须分配一个新数组）。 因此调用者可以自由修改返回的数组。
T[]	toArray(T[] a)|返回一个包含此set中所有元素的数组; 返回数组的运行时类型是指定数组的运行时类型。 如果集合适合指定的数组，则返回指定类型的数组。 否则，将使用指定数组的运行时类型和此set的大小分配新数组。如果此set适合指定的数组，并且有空余空间（即，数组的元素多于此set），则紧跟在该set结尾之后的数组中的元素将设置为null。

🌂从接口 java.util.Collection 继承的方法有：
parallelStream, removeIf, stream

🌂从接口 java.lang.Iterable 继承的方法有：
forEach

## 4、Java Array to Set

与List不同，我们不能直接将Java Set转换为数组，因为它不是使用Array实现的。因此我们不能直接使用 Arrays 将数组转换为 Set 。 但是，我们可以采用另一种方法，我们可以使用Arrays.asList（）方法将数组转换为List，然后使用它来创建Set。

通过使用这种方法，我们可以通过两种方式将Java数组转换为Set。 下面让我们用一个简单例子，分别演示以下这两种方式。

### 4.1、方法1

在这种方法中，首先我们需要使用给定的数组创建一个List，然后使用它来创建一个Set，如下所示。
```java
 public static void main (String[] args) {
       String[]a={"h","e","l","l","o"};
        Set<String> b=new HashSet<>(Arrays.asList(a));
        System.out.println(b);
    }
```
### 4.2、方法2

在这种方法中，我们不使用 List 来作为从Array 转换为 Set 的中间集合。 首先创建一个空的 HashSet，然后使用 Collections.addAll（）将数组元素复制到给定的 Set 中，如下所示。
```java
  public static void main (String[] args) {
        String[]a={"h","e","l","l","o"};
        Set<String> b=new HashSet<>();
        Collections.addAll(b,a);
        System.out.println(b);
    }
```
## 5、Java Set to Array

接下来我们将使用 Set.toArray（）方法将一组字符串转换为字符串数组，如下所示。
```java
    public static void main (String[] args) {
        Set<String> a=new HashSet<>();
        a.add("h");
        a.add("e");
        a.add("l");
        a.add("l");
        a.add("o");
        //这一步就是将set转换成array
        String str[]=a.toArray(new String[a.size()]);
        System.out.println(Arrays.toString(str));
    }

    /**
     *
     * 运行结果
     * [e, h, l, o]
     *
     */
```
## 6、Java Set Sorting

我们知道，Set（HashSet）不支持直接对元素排序。 它以随机顺序存储和显示元素。但是，我们有一些方法可以对它的元素进行排序，如下所示：
```java
ublic static void main (String[] args) {
        Set<Integer> intsSet=new HashSet<>();
        Random random=new Random();
        for (int i = 0; i <10 ; i++) {
            intsSet.add(random.nextInt(1000));
        }
        System.out.println("无序："+intsSet);
        Set<Integer> youxu=new TreeSet<>(intsSet);
        System.out.println("有序："+youxu);
    }
```
**输出结果：**
```java
无序：[371, 390, 408, 41, 329, 410, 588, 316, 573, 205]
有序：[41, 205, 316, 329, 371, 390, 408, 410, 573, 588]

```
## 7、Java Set to Stream

下面是一个示例，说明如何将Java Set转换为Stream并根据我们的要求执行某些操作。
```java
  public static void main (String[] args) {
        Set<String> a=new HashSet<>();
        a.add("h");
        a.add("e");
        a.add("l");
        a.add("l");
        a.add("o");
        //将set转换为stream并且进行遍历输出
        a.stream().forEach(System.out::println);
    }
    /**
     * 输出结果：
     * e
     * h
     * l
     * o
     */
```
## 8、Java Set 常用操作

在Java Set上我们最常用的操作是**add**，**addAll**，**clear**，**size**等。下面是一个简单的Java Set 示例，显示了常用的方法用法。
```java
   public static void main (String[] args) {
        Set<String> a=new HashSet<>();
        a.add("H");
        a.add("E");
        a.add("L");
        System.out.println("a:"+ Arrays.toString(a.toArray()));
        Set<String> b=new HashSet<>();
        b.add("O");
        b.add("U");
        //将b中的元素全部添加到a中
        a.addAll(b);
        System.out.println("将b中的元素全部添加到a中:"+Arrays.toString(a.toArray()));
        //清空
        b.clear();
        //输出a的长度
        System.out.println("输出b的长度:"+a.size());
        //这里清空a
        a.clear();
        a.add("H");
        a.add("W");
        //这里用contains方法看一下是否包含某一个元素
        System.out.println("判断是否包含W："+a.contains("W"));

    }
```
**输出结果：**
```java
a:[E, H, L]
将b中的元素全部添加到a中:[E, U, H, L, O]
输出b的长度:5
判断是否包含W：true

```
## 9、Java Set Iterator
下面是一个例子，展示了如何迭代Java Set。
```java
 public static void main(String[] args) {

        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < 5; i++) {
            set.add(i);
        }

        //获取迭代器
        Iterator iterator = set.iterator();

        //简单迭代
        System.out.println("迭代集合元素：");
        while (iterator.hasNext()) {
            int i = (int) iterator.next();
            System.out.print(i + " ");
        }

        //使用迭代器修改 set 元素
        iterator = set.iterator();
        while (iterator.hasNext()) {
            int x = (int) iterator.next();
            if (x % 2 == 0) {
                iterator.remove();
            }
        }
        System.out.println();
        System.out.println("使用迭代器修改set元素以后：" + set);

    }
```
**输出结果：**
```java
迭代集合元素：
0 1 2 3 4 
使用迭代器修改set元素以后：[1, 3]
```


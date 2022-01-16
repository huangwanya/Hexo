---
title: Java集合框架（八）：List 源码分析
date: 2020-04-03 21:05:20
tags: 框架
categories: java集合框架
---
## 1、List 简述
Java.util.List 是 Collection 的子接口。它是一个有序集合，可以存储重复的值。由于List保留可插入元素的位置的控制，因此它可以按索引访问元素，也可以搜索列表中的元素。

关于Java List的一些重要特点有：
🌂Java List 接口是 Java Collections Framework 的成员。
🌂List 允许添加重复元素。
🌂List允许拥有’null’元素。
🌂List接口在Java 8中有许多默认方法，例如replaceAll，sort和spliterator。
🌂列表索引从0开始，就像数组一样。
🌂List支持泛型，我们应尽可能使用它。 将Generics与List一起使用将在运行时避免ClassCastException。

## 2、List 类图2、List 类图
Java List接口扩展了 Collection 接口。 Collection 接口又扩展了 Iterable接口。
![](https://s1.ax1x.com/2020/04/03/GagEqO.png)
一些最常用的 List 实现类有 ArrayList，LinkedList，Vector，Stack，CopyOnWriteArrayList。
AbstractList 提供了List接口的重要方法实现，以减少实现List的工作量。

## 3、List 方法
### 3.1、List 中常用方法如下

方法  | 方法描述
------------- | -------------
int size()|获取列表中的元素数量，如果此列表元素个数大于 Integer.MAX_VALUE，则返回 Integer.MAX_VALUE。
boolean isEmpty()|检查列表是否为空。
boolean contains(Object o)|如果此列表包含指定的元素，则返回true。
Iterator iterator()|以适当的顺序返回此列表中元素的迭代器。
Object[] toArray()|以适当的顺序返回包含此列表中所有元素的数组
T[] toArray(T[] a)|以适当的顺序返回包含此列表中所有元素的数组; 返回数组的类型是指定数组的运行时类型。
boolean add(E e)|将指定的元素追加到此列表的末尾。
boolean remove(Object o)|从此列表中删除第一次出现的指定元素。
boolean containsAll(Collection<?> c)|如果此列表包含指定集合的所有元素，则返回true。
boolean addAll(Collection<? extends E> c)|将指定集合中的所有元素按指定集合的迭代器（可选操作）返回的顺序追加到此列表的末尾。如果操作正在进行时修改了指定的集合，则此操作的行为是不确定的。
boolean addAll(int index, Collection<? extends E> c)|将指定集合中的所有元素插入到此列表指定位置。将当前位置的元素（如果有）和任何后续元素向右移动。新元素将按照指定集合的迭代器返回的顺序出现在此列表中。如果在操作正在进行时修改了指定的集合，则此操作的行为是不确定的。
boolean removeAll(Collection<?> c)|从此列表中删除指定集合中包含的所有元素。
boolean retainAll(Collection<?> c)|仅保留此列表中包含在指定集合中的元素。
void clear()|从列表中删除所有元素。
E get(int index)|返回列表中指定位置的元素。
void add(int index, E element)|将指定元素插入此列表中的指定位置。 将当前位于该位置的元素（如果有）和任何后续元素向右移动。
E remove(int index)|删除此列表中指定位置的元素。 将任何后续元素向左移位。 返回从列表中删除的元素。
int indexOf(Object o)|返回此列表中第一次出现的指定元素的索引，如果此列表不包含该元素，则返回-1。
int lastIndexOf(Object o)|返回此列表中指定元素最后一次出现的索引，如果此列表不包含该元素，则返回-1。
E set(int index, E element)|用指定的元素替换列表中指定位置的元素。
ListIterator listIterator()|从列表中的指定位置开始，返回列表中元素的列表迭代器（按正确顺序）。 指定的索引指示初始调用next时将返回的第一个元素。 对previous的初始调用将返回指定索引减去1的元素。
List subList(int fromIndex, int toIndex)|返回指定 fromIndex（包含）和 toIndex（不包含）之间此列表部分的元素。如果fromIndex和toIndex相等，则返回的列表为空。

### 3.2、在Java 8中新增加的默认方法如下
1、default void replaceAll(UnaryOperator operator)： 将该列表的每个元素替换为将运算符应用于该元素的结果。 当抛出错误或运行时异常时，将返回给调用者。

实现要求：
对于此列表，默认实现等效于：

```java
final ListIterator<E> li = list.listIterator();
     while (li.hasNext()) {
         li.set(operator.apply(li.next()));
     }
```
如果列表的 list-iterator 不支持set操作，则在替换第一个元素时将抛出UnsupportedOperationException。

2、default void sort(Comparator<? super E> c)： 根据指定的比较器对此列表进行排序。此列表中的所有元素必须使用指定的比较器进行相互比较（即，c.compare（e1，e2）不得为列表中的任何元素e1和e2抛出 ClassCastException）。如果指定的比较器为null，则此列表中的所有元素都必须实现Comparable接口，并且应使用元素的自然顺序。默认实现获取包含此列表中所有元素的数组，对数组进行排序，并迭代此列表，从数组中的相应位置重置每个元素。 （这样可以避免因尝试对列表进行排序而导致 n^2 log(n) 的性能。）

3、default Spliterator spliterator()： 在此列表中的元素上创建Spliterator。

## 4、Java Array to List
我们可以使用Arrays 类将数组转换为列表，但是我们无法对列表进行任何结构修改，它将抛出 java.lang.UnsupportedOperationException。所以最好的方法是使用for循环来迭代数组来创建列表。

下面是一个简单示例，展示了如何正确地将java数组转换为列表。
```java
 public static void main (String[] args) {
        String[]hello={"h","e","l","l","o"};
        List<String> a= Arrays.asList(hello);
        System.out.println("Arrays.asList 数组转为列表："+a);
        /**
         * List是由数组支持的，我们不能做结构修改
         * 以下两个语句都将抛出java.lang.UnsupportedOperationException
         */
        //hellosList.remove("l");
        //hellosList.clear();

        //使用for循环将元素从数组复制到列表，可以安全地修改列表
        List<String> b=new ArrayList<>();
        for(String s:hello){
            b.add(s);

        }
        System.out.println("for遍历元素添加到新的列表中："+b);
        b.clear();


    }
```
**输出结果：**
```java
Arrays.asList 数组转为列表：[h, e, l, l, o]
for遍历元素添加到新的列表中：[h, e, l, l, o]
```
## 5、Java List to Array
一个简单的示例，将列表转换为数组的正确方法。
```java
 public static void main (String[] args) {
        List<String> a=new ArrayList<>();
        a.add("H");
        a.add("W");

        //将列表转为数组
        String[] b=new String[a.size()];
        b=a.toArray(b);
        System.out.println(Arrays.toString(b));
    }
```
**输出结果：**

```java
[H, W]
```
## 6、Java List sort
有两种方法可以对列表进行排序。我们可以使用 Collections 类进行自然排序，或者我们可以使用List sort（）方法并使用我们自己的 Comparator 进行排序。

下面是java列表排序的一个简单示例：
```java
public static void main (String[] args) {
        List<Integer> a=new ArrayList<>();
        Random random=new Random();
        for (int i = 0; i < 10; i++) {
            a.add(random.nextInt(1000));
        }
        //使用Collections 对列表进行排序
        Collections.sort(a);
        System.out.println("Collections 自然排序："+a);

        //自定义比较器，逆序排序
        a.sort((o1,o2)->{
            return(o2-o1);
        });
        System.out.println("自定义比较器，逆序排序:"+a);
    }
```
**输出结果：**
```java
Collections 自然排序：[6, 141, 223, 334, 357, 475, 511, 647, 948, 996]
自定义比较器，逆序排序:[996, 948, 647, 511, 475, 357, 334, 223, 141, 6]
```
## 7、Java List 常用操作方法
在java列表上执行的最常见操作是**add**, **remove**, **set**, **clear**, **size** 等。

下面一个简单的java列表示例，展示了常用的方法用法：
```java
 public static void main (String[] args) {
        List<String> a=new ArrayList<>();

        //add
        a.add("H");
        a.add("W");

        //将元素插入到指定位置
        a.add(1,"E");
        System.out.println("将元素插入到指定位置:"+a);

        List<String > b=new ArrayList<>();
        b.add("L");
        b.add("W");
        b.add("W");

        //将b集合中所有的元素追加到a集合中
        a.addAll(b);
        System.out.println("将b集合中所有的元素追加到a集合中:"+a);

        //清空集合
        b.clear();

        //集合元素大小
        System.out.println("集合a元素大小"+a.size());

        //替换指定位置的元素
        a.set(1,"W");
        System.out.println("将索引位置为1的元素替换为 W："+a);

        //subList
        a.clear();
        a.add("L");
        a.add("O");
        a.add("V");
        a.add("E");
        ////截取集合 a 中索引位置从 0 到 索引位置为 1 的元素
        b=a.subList(0,2);
        System.out.println("a.subList(0,2)"+b);
        ////替换集合 a 中索引位置为 0 的元素
        a.set(0,"W");
        System.out.println("替换集合 a 中索引位置为 0 的元素为'W'"+a);

        //添加
        System.out.println("b集合:"+b);
        b.add("G");
        System.out.println("b集合中添加G："+b);
    }
```
**输出结果：**
```java
将元素插入到指定位置:[H, E, W]
将b集合中所有的元素追加到a集合中:[H, E, W, L, W, W]
集合a元素大小6
将索引位置为1的元素替换为 W：[H, W, W, L, W, W]
a.subList(0,2)[L, O]
替换集合 a 中索引位置为 0 的元素为'W'[W, O, V, E]
b集合:[W, O]
b集合中添加G：[W, O, G]
```
## 8、Java List iterator

下面是一个简单的例子，展示了如何在java中迭代列表。
```java
    public static void main(String[] args) {

        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            list.add(i);
        }

        Iterator<Integer> iterator = list.iterator();

        //简单遍历
        System.out.print("简单迭代遍历：");
        while (iterator.hasNext()) {
            int i = iterator.next();
            System.out.print(i + ", ");
        }

        //使用迭代器修改列表
        iterator = list.iterator();
        while (iterator.hasNext()) {
            int x = iterator.next();
            if (x % 2 == 0) {
                iterator.remove();
            }
        }
        System.out.println();
        System.out.println("使用迭代器删除偶数："+list);

        //迭代时改变列表结构
        iterator = list.iterator();
        while (iterator.hasNext()) {
            int x = iterator.next();
            //ConcurrentModificationException
            if (x == 1) {
                list.add(10);
            }
        }
    }


```
**输出结果：**
```java
简单迭代遍历：0, 1, 2, 3, 4, 
使用迭代器删除偶数：[1, 3]
Exception in thread "main" java.util.ConcurrentModificationException
	at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:909)
	at java.util.ArrayList$Itr.next(ArrayList.java:859)
	at com.lkf.collection.list.ListIteratorExample.main(ListIteratorExample.java:44)

```
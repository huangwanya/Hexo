---
title: Java集合框架（九）：Queue 源码分析
date: 2020-04-04 17:39:20
tags: 框架
categories: java集合框架
---
## 1、Queue 简述1、Queue 简述
**队列**是一种**特殊的线性表**，它只允许在表的**前端**（front）进行**删除**操作，而在表的**后端**（rear）进行**插入**操作。进行插入操作的端称为队尾，进行删除操作的端称为队头。队列中没有元素时，称为空队列。

Java Queue是java.util包中提供的接口，并扩展了java.util.Collection接口。
就像Java List一样，Java Queue是有序元素（或对象）的集合，但它以不同方式执行插入和删除操作。 在处理这些元素之前，我们可以使用Queue存储元素。

![](https://s1.ax1x.com/2020/04/04/GwURzD.png)

🌂java.util.Queue接口是java.util.Collection接口的子类型。

🌂就像现实世界的排队（例如，在银行或ATM中）一样，Queue在队列的末尾插入元素并从队列的开头删除元素。

🌂Java Queue遵循FIFO顺序来插入和删除它的元素。 FIFO代表先入先出。

🌂Java Queue支持Collection接口的所有方法。

🌂最常用的Queue实现是LinkedList，ArrayBlockingQueue和PriorityQueue。

🌂BlockingQueues不接受null元素。 如果我们执行任何与null相关的操作，它将抛出NullPointerException。

🌂BlockingQueues用于实现基于生产者/消费者的应用程序。

🌂BlockingQueues是线程安全的。

🌂java.util包中可用的所有队列都是无界队列，java.util.concurrent包中可用的队列是有界队列。

🌂所有Deques都不是线程安全的。

🌂ConcurrentLinkedQueue 是一个基于链表的无界线程安全队列。

🌂除了Deques之外，所有队列都支持在队列尾部插入并在队列的头部删除。

🌂Deques 是双端队列，它支持在队列两端插入和移除元素。

![](https://s1.ax1x.com/2020/04/04/GwUgJK.png)
## 2、Queue 类图
![](https://s1.ax1x.com/2020/04/04/GwUodI.jpg)
Java Queue 接口**扩展了 Collection** 接口。 Collection 接口扩展了 Iterable 接口。 一些常用的Queue实现类是 LinkedList，PriorityQueue，ArrayBlockingQueue，DelayQueue，LinkedBlockingQueue，PriorityBlockingQueue 等.AbstractQueue 提供了 Queue 接口主要方法的实现，以减少实现 Queue 的工作量。

## 3、Queue 方法简述
队列通常（但不一定）以FIFO（**先进先出**）方式对元素进行排序。例外的是优先级队列，它根据提供的比较器对元素进行排序，或者元素的自然排序，以及LIFO队列（或**堆栈**），它们对元素LIFO（**后进先出**）进行排序。

🌂无论使用什么顺序，队列的头部都是通过调用remove（）或poll（）来删除的元素。 在FIFO队列中，所有新元素都插入队列的尾部。

其他类型的队列可能使用不同的放置规则。 每个Queue实现都必须指定其排序属性。

Queue接口**不定义阻塞队列**方法，这在并发编程中很常见。 这些等待元素出现或空间可用的方法在BlockingQueue接口中定义，该接口扩展了此接口。

队列实现通常**不允许插入null元素**，尽管某些实现（如LinkedList）不禁止插入null。 即使在允许它的实现中，也不应将null插入到Queue中，因为null也被poll方法用作特殊返回值，以指示队列不包含任何元素。

下面我们将讨论一些有用且经常使用的 Java Queue 方法：

方法  | 方法描述
------------- | -------------
boolean add(E e)|如果可以在不违反容量限制的情况下立即执行此操作，则将指定的元素插入此队列，成功时返回true，如果当前没有可用空间则抛出IllegalStateException。
boolean offer(E e)|如果可以在不违反容量限制的情况下立即执行此操作，则将指定的元素插入此队列，成功时返回true，如果当前没有可用空间则返回 false。
E remove()|检索并删除此队列的头部元素。 此方法与poll的不同之处仅在于，如果此队列为空，则抛出异常 NoSuchElementException。
E poll()|检索并删除此队列的头部，如果此队列为空，则返回null。
E element()|检索但不删除此队列的头部。 此方法与peek的不同之处仅在于，如果此队列为空，则抛出异常 NoSuchElementException。
E peek()|检索但不移除此队列的头部，如果此队列为空，则返回null。

## 4、Java Array to Queue
在这里，我们将通过一个简单的例子演示如何使用“Collections.addAll（）”方法将Java数组转换为Queue。
```java
 public static void main(String[] args) {

        String nums[] = {"one", "two", "three", "four", "five"};
        Queue<String> queue = new LinkedList<>();
        Collections.addAll(queue, nums);
        System.out.println(queue);
    }

```
**输出结果：**
```java
[one, two, three, four, five]

```
## 5、Java Queue to Array
在这里，我们将通过一个简单的例子演示如何使用“**toArray（）**”将Java队列转换为Java数组。
```java
public static void main(String[] args) {

        Queue<String> queue = new LinkedList<>();
        queue.add("one");
        queue.add("two");
        queue.add("three");
        queue.add("four");
        queue.add("five");

        String strArray[] = queue.toArray(new String[queue.size()]);
        System.out.println("队列转换为数组：" + Arrays.toString(strArray));

    }

```
**输出结果：**
```java
队列转换为数组：[one, two, three, four, five]
```
## 6、Java队列常用操作

Java Queue支持Collection接口支持的**所有**操作以及更多操作。 它以两种形式支持几乎所有操作：

🌂一种在操作失败时抛出异常，另一种返回特殊值（null或false，具体取决于操作）。

🌂一种形式的插入操作专门用于容量限制的队列实现，在大多数实现中，插入操作不会失败。

队列方法的摘要

|Operation（操作）|Throws exception（抛出异常）|Returns special value（返回指定值） |
| ------------- | ------------- | ------------- |
| Insert（插入） | add(e)  |offer(e)
|Remove（删除）|remove()|poll()
|Examine（检查）|element()|peek()


🌂🌂🌂接下来我们将用示例演示每种操作方法的使用。
### 6.1、Java 队列插入操作
#### 6.1.1、add()

**add（）**操作用于将新元素插入队列。 如果它成功执行插入操作，则返回“**true**”值。 否则抛出**java.lang.IllegalStateException**。
```java
public static void main(String[] args) {
        BlockingQueue<String> queue = new ArrayBlockingQueue<>(2);

        System.out.println(queue.add("one"));
        System.out.println(queue.add("two"));
        System.out.println("队列中的元素："+queue);
    }

```
**输出结果：**
```java
true
true
队列中的元素：[one, two]
```
由于我们的队列仅限于两个元素，当我们尝试使用BlockingQueue.add（）添加第三个元素时，它会抛出异常，如上所示。

#### 6.1.2、offer()

offer（）操作用于将新元素插入队列。 如果它成功执行插入操作，则返回“true”值。 否则返回“false”值。
```java
public static void main(String[] args) {

        BlockingQueue<String> queue = new ArrayBlockingQueue<>(2);

        System.out.println(queue.offer("one"));
        System.out.println(queue.offer("two"));
        System.out.println("队列中的元素："+queue);
        System.out.println("队列已满，再次offer元素："+queue.offer("three"));
    }

```
**输出结果：**
```java
true
true
队列中的元素：[one, two]
队列已满，再次offer元素：false

```

### 6.2、Java 队列删除操作

#### 6.2.1、remove()

**remove（）**操作用于从队列头部删除元素。 如果它成功执行删除操作，则**返回队列的head元素**。 否则抛出**java.util.NoSuchElementException**。

```java
 public static void main(String[] args) {
        Queue<String> queue = new LinkedList<>();
        queue.offer("one");
        queue.offer("two");
        System.out.println("队列中的元素：" + queue);
        System.out.println("删除第一个元素：" + queue.remove());
        System.out.println("删除第二个元素：" + queue.remove());
        System.out.println("删除第三个元素：" + queue.remove());
    }
```
**输出结果：**
```java
队列中的元素：[one, two]
删除第一个元素：one
删除第二个元素：two
Exception in thread "main" java.util.NoSuchElementException
	at java.util.LinkedList.removeFirst(LinkedList.java:270)
	at java.util.LinkedList.remove(LinkedList.java:685)
	at com.lkf.collection.queue.QueueRemoveOperation.main(QueueRemoveOperation.java:20)
```
由于我们的队列只有两个元素，当我们尝试第三次调用remove（）方法时，它会抛出一个**异常**，如上所示。

#### 6.2.2、poll()
**poll（）**操作用于从队列头部删除元素。 如果它成功执行删除操作，则**返回队列的head元素**。 否则返回“**null**”值。
```java
 public static void main(String[] args) {
        Queue<String> queue = new LinkedList<>();
        queue.offer("one");
        queue.offer("two");
        System.out.println("队列中的元素：" + queue);
        System.out.println("删除第一个元素：" + queue.poll());
        System.out.println("删除第二个元素：" + queue.poll());
        System.out.println("删除第三个元素：" + queue.poll());
    }
```
**输出结果：**
```java
队列中的元素：[one, two]
删除第一个元素：one
删除第二个元素：two
删除第三个元素：null
```
由于我们的队列只有两个元素，当我们尝试第三次调用poll（）方法时，它返回null值，如上所示。

### 6.3、Java 队列检索操作

#### 6.3.1、element()

**element（）**操作用于从队列头部检索元素，而不删除它。 如果它成功执行检查操作，则**返回队列的head元素**。 否则抛出**java.util.NoSuchElementException**。
```java
public static void main(String[] args) {

        Queue<String> queue = new LinkedList<>();
        queue.add("one");

        System.out.println(queue.element());
        System.out.println(queue);
        queue.clear();
        System.out.println(queue.element());
    }

```
**输出结果：**
```java
one
[one]
Exception in thread "main" java.util.NoSuchElementException
	at java.util.LinkedList.getFirst(LinkedList.java:244)
	at java.util.LinkedList.element(LinkedList.java:663)
	at com.lkf.collection.queue.QueueElementOperation.main(QueueElementOperation.java:21)
```

如果我们尝试在空Queue上调用element（）方法，它会抛出异常，如上所示。

#### 6.3.2、peek()

**peek（）**操作用于从队列头部检索元素，而不删除它。 如果它成功执行检查操作，则**返回队列的head元素**。 否则返回**null**值。
```java
 public static void main(String[] args) {

        Queue<String> queue = new LinkedList<>();
        queue.add("one");

        System.out.println(queue.peek());
        System.out.println(queue);
        queue.clear();
        System.out.println(queue.peek());
    }
```
**输出结果：**
```java
one
[one]
null
```
如果我们尝试在空Queue上调用peek（）方法，它将返回null值，但不会抛出异常，如上所示。

### 6.4、Java 队列常用操作方法的区别

#### 6.4.1、add 与 offer 区别
add 和 offer 方法都是向队列中添加一个元素。
当一个大小受限制的队列满时，使用 add 方法将会抛出一个 unchecked 异常；使用 offer 方法会返回 false。

#### 6.4.2、remove 与 poll 区别
🌂remove() 和 poll() 方法都是删除并返回队列头部的第一个元素。确切地说，从队列中删除哪个元素是队列排序策略的一个功能，该策略因实现而异。
🌂remove（）和 poll（）方法的不同之处仅在于队列为空时的行为：remove（）方法抛出异常，而poll（）方法返回null。

#### 6.4.3、element 与 peek 区别

element（）和 peek（）方法返回但不删除队列的头部元素。与 remove() 方法类似，在队列为空时， element() 抛出一个异常，而 peek() 返回 null。

## 7、Java 队列分类

除了以下队列的分类，还有些队列是 Deques，有些队列是PriorityQueues。另外，java.util 包中可用的所有队列都是无界队列，java.util.concurrent 包中可用的队列都是有界队列。

### 7.1、有界队列（Bounded Queues） 和 无界队列（Unbounded Queues）

在Java中，我们可以找到很多 Queue 实现。 我们可以将它们大致分为以下两种类型：
有界队列 和 无界队列

#### 7.1.1、有界队列（Bounded Queues）

有界队列是有容量限制的队列，这意味着我们需要在创建时设置队列的最大大小。 例如：ArrayBlockingQueue。

#### 7.1.2、无界队列（Unbounded Queues）

无界队列是不受容量限制的队列，这意味着我们不需要设置队列的大小。 例如：LinkedList

### 7.2、阻塞队列（BlockingQueue） 和 非阻塞队列（Non-Blocking Queues）

在其他方面，我们可以将它们大致分为以下两种类型：阻塞队列、非阻塞队列

#### 7.2.1、阻塞队列（BlockingQueue）

实现 BlockingQueue 接口的所有队列都是 阻塞队列，其余的都是 非阻塞队列。

BlockingQueues阻塞直到它完成它的工作或超时，但Non-BlockingQueues不会。

##### 7.2.1.1、BlockingQueue 操作

除了Queue的两种操作形式之外，BlockingQueue 还支持另外两种形式，如下所示：


|Operation|Throws exception|Special value|Blocks|Times out|
| ------ |  ------ | ------ | ------ | ------ |
|Insert|add(e)|offer(e)|put(e)|offer(e, time, unit)|
|Remove|remove()|poll()|take()|poll(time, unit)|
|Examine|element()|peek()|N/A|N/A|

#### 7.2.2、非阻塞队列（Non-Blocking Queues）

非实现 BlockingQueue 接口的所有队列都是 非阻塞队列。

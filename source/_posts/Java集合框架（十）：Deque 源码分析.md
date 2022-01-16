---
title: Java集合框架（十）：Deque 源码分析
date: 2020-04-04 18:13:02
tags: 框架
categories: java集合框架
---
## 1、Deque 简述
线性集合，支持两端插入和移除元素。名称deque是“**双端队列**（double ended queue）”的缩写，通常发音为“deck”。

大多数Deque实现对它们可能**包含的元素数量没有固定限制**，但是此接口支持容量限制的deques以及没有固定大小限制的deques。此接口定义了访问双端队列两端元素的方法，提供了插入，移除和检索元素的方法。这些方法中的每一种都以两种形式存在：一种在操作失败时**抛出异常**，另一种返回特殊值（**null或false**，具体取决于操作）。 后一种形式的插入操作专门设计用于容量限制的Deque实现， 在大多数实现中，插入操作不会失败。

**上述中三种操作涉及十二种方法总结在下表中：**
![](https://s1.ax1x.com/2020/04/04/GwO6KK.png)
## 2、Deque 类图
![](https://s1.ax1x.com/2020/04/04/GwUsd1.png)

🌂父接口： Collection, Iterable, Queue

🌂子接口： lockingDeque

🌂实现类： ArrayDeque, ConcurrentLinkedDeque, LinkedBlockingDeque, LinkedList

此接口**扩展**了Queue接口。 当deque用作队列时，会产生FIFO（**先进先出**）行为。元素在**双端队列的末尾添加并队头开始删除**。

从 Queue 接口继承的方法与 Deque 完全等效的方法，如下表所示：

**  Queue和Deque方法的比较**
		
|Queue 方法|等价 Deque 方法|
| ------ |  ------ |
|add(e)|addLast(e)|
|offer(e)|offerLast(e)|
|remove()|removeFirst()|
|poll()|pollFirst()|
|element()|getFirst()|
|peek()|peekFirst()|

Deques也可以用作LIFO（**后进先出**）**堆栈**。 应优先使用此接口，而不是传统的Stack类。当deque用作堆栈时，元素将从双端队列的队头弹出元素。

堆栈方法与Deque完全等效的方法，如下表所示：
**Stack和Deque方法的比较**

|Stack 方法|等价 Deque 方法|
| ------ |  ------ |
|push(e)|addFirst(e)|
|pop()|removeFirst()|
|peek()|peekFirst()|

此接口提供了两种方法来删除内部元素，removeFirstOccurrence和removeLastOccurrence。

**与List接口不同，此接口不支持对元素的索引访问。**

## 3、Deque 方法概述

|方法|作用|
| ------ |  ------ |
|void addFirst(E e)|如果可以在不违反容量限制的情况下立即插入指定元素，则在此双端队列的前面插入指定元素，如果当前没有可用空间则抛出IllegalStateException。当使用容量限制的双端队列时，通常最好使用方法offerFirst（E）。
|void addLast(E e)|如果可以在不违反容量限制的情况下立即插入指定元素，则在此双端队列的末尾插入指定元素，如果当前没有可用空间则抛出IllegalStateException。 当使用容量限制的双端队列时，通常最好使用方法offerLast（E）。
|boolean offerFirst(E e)|将指定元素插入此双端队列的前面，除非它违反容量限制。 使用容量限制的双端队列时，此方法通常优于addFirst（E）方法，如果元素已添加到此双端队列，则返回true，否则返回false。
|boolean offerLast(E e)|在此双端队列的末尾插入指定的元素，除非它违反容量限制。 使用容量限制的双端队列时，此方法通常优于addLast（E）方法，，如果元素已添加到此双端队列，则返回true，否则返回false。
|E removeFirst()|检索并删除此双端队列的第一个元素。 此方法与pollFirst的不同之处仅在于，如果此双端队列为空，则抛出异常。
|E removeLast()|检索并删除此双端队列的最后一个元素。 此方法与pollLast的不同之处仅在于，如果此双端队列为空，则会抛出异常。
|E pollFirst()|检索并删除此双端队列的第一个元素，如果此双端队列为空，则返回null。
|E pollLast（）|检索并删除此双端队列的最后一个元素，如果此双端队列为空，则返回null。
|E getFirst()|检索但不删除此双端队列的第一个元素。 此方法与peekFirst的不同之处仅在于，如果此双端队列为空，则抛出异常。
|E getLast()|检索但不删除此双端队列的最后一个元素。 此方法与peekLast的不同之处仅在于，如果此双端队列为空，则会抛出异常。
|E peekFirst()|检索但不删除此双端队列的第一个元素，如果此双端队列为空，则返回null。
|E peekLast()|检索但不删除此双端队列的最后一个元素，如果此双端队列为空，则返回null。
|boolean removeFirstOccurrence(Object o)|从此双端队列中删除第一次出现的指定元素。 如果双端队列不包含该元素，则不会更改。如果此双端队列包含指定的元素，则返回true。
|boolean removeLastOccurrence(Object o)|从此双端队列中删除最后一次出现的指定元素。 如果双端队列不包含该元素，则不会更改。如果此双端队列包含指定的元素，则返回true。
|boolean add(E e)|如果可以在不违反容量限制的情况下立即执行此操作，则将指定的元素插入此双端队列的尾部，成功时返回true，如果当前没有空间则抛出IllegalStateException。 当使用容量限制的双端队列时，通常优先使用 offer。此方法等同于addLast（E）。
|boolean offer(E e)|如果可以在不违反容量限制的情况下立即执行此操作，则将指定的元素插入此双端队列的尾部，成功时返回true，如果当前没有可用空间则返回false。 使用容量限制的双端队列时，此方法通常优于add（E）方法，此方法相当于offerLast（E）。
|E remove()|检索并删除此双端队列表示的队列的头部（换句话说，此双端队列的第一个元素）。 此方法与poll的不同之处仅在于，如果此双端队列为空，则抛出异常。此方法等同于removeFirst（）。
|E poll()|检索并删除此双端队列的头部（换句话说，此双端队列的第一个元素），如果此双端队列为空，则返回null。此方法等同于pollFirst（）。
|E element()|检索但不删除此双端队列表示的队列的头部（换句话说，此双端队列的第一个元素）。 此方法与peek的不同之处仅在于，如果此双端队列为空，则抛出异常。此方法等同于getFirst（）。
|E peek()|检索但不删除此双端队列表示的队列的头部（换句话说，此双端队列的第一个元素），如果此双端队列为空，则返回null。此方法等效于peekFirst（）。
|void push(E e)|如果可以在不违反容量限制的情况下立即执行此操作，则将元素压入到，在此双端队列的头部，如果当前没有可用空间，则抛出IllegalStateException。此方法等同于addFirst（E）。
|E pop()|从此双端队列表示的堆栈中弹出一个元素。 换句话说，删除并返回此双端队列的第一个元素。此方法等同于removeFirst（）。
|boolean remove(Object o)|从此双端队列中删除第一次出现的指定元素。 如果双端队列不包含该元素，则不会更改。 如果此双端队列包含指定的元素，则返回true。此方法等同于removeFirstOccurrence（Object）。
|boolean contains(Object o)|如果此双端队列包含指定的元素，则返回true。
|int size()|返回此双端队列中的元素数。
|Iterator iterator()|以适当的顺序返回此双端队列中元素的迭代器。 元素将从第一个（头部）到最后一个（尾部）按顺序返回。
|Iterator descendingIterator()|以相反的顺序返回此双端队列中元素的迭代器。 元素将按从最后（尾部）到第一个（头部）的顺序返回。
---
title: Java集合框架（十八）：HashMap 源码分析
date: 2020-04-12 19:37:13
tags: 框架
categories: java集合框架
---
## HashMap简述

**HashMap**是工作中最常用的集合工具之一，在整个集合框架中也是很重要的一部分，因此本篇文章主要讲述它的底层实现原理，因为jdk1.8中对HashMap的数据结构有了修改，所以本篇将会分别讲解**jdk1.7**和**jdk1.8**中HashMap的区别，通过对比学习来加深对HashMap的理解

jdk1.8之**前**HashMap采用【**数组+链表**】实现，使用链表处理hash冲突，同一个hash值都存在一个链表里。但是当存储的元素较多时，hash值相等的元素也会增多，通过key值依次查找的效率就降低了许多。

jdk1.8**中**，HashMap采用【**数组+链表+红黑树**】实现，当链表长度超过8时，将链表转换为红黑树，这样就大大提高的查找的时间

## HashMap数据结构

### JDK1.7中HashMap数据结构
![](https://s1.ax1x.com/2020/04/12/GOn0kn.png)

HashMap中的数组即为嵌套类Entry，数组中的每个元素是一个单项链表，每个Entry包含**四个**属性：
🌂key, value, hash 值和用于单向链表的 next。

🌂capacity：当前数组容量，始终保持 2^n，可以扩容，扩容后数组大小为当前的 2 倍。

🌂loadFactor：负载因子，默认为 0.75。

🌂threshold：扩容的阈值，等于 capacity * loadFactor

```java
static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        int hash;
      .....................................
  }
```
### JDK1.8中HashMap数据结构

![](https://s1.ax1x.com/2020/04/12/GOndTs.png)


**Java8 **对 HashMap 做了一些**修改**，最大的不同就是利用了红黑树，所以其由 【**数组+链表+红黑树**】 组成。

我们知道，Java7 HashMap查找的时候，根据 hash 值我们能够快速定位到数组的具体**下标**，但是之后，需要顺着链表一个个比较下去才能找到我们需要的，时间复杂度取决于**链表的长度**，为 O(n)。

为了**降低**这部分的**开**销，在 Java8 中，当链表中的元素超过了 8 个以后，会将链表转换为**红黑树**，在进行查找的时候可以降低时间复杂度为 O(logN)。

Java7 中使用 Entry 来代表每个 HashMap 中的数据元素，Java8 中使用 Node，都是** key**，**value**，**hash** 和 **next** 这**四个**属性，不过，Node 只能用于链表的情况，红黑树的情况需要使用 TreeNode

```java
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
    ...............................................
}
```
## HashMap存取值分析

###  HashMap存值（put）分析【JDK1.7】

#### HashMap存值源码走读【JDK1.7】

transient Entry<K,V>[] table = (Entry<K,V>[]) EMPTY_TABLE;

```java
public V put(K key, V value) {
        //先判断数组是否为空，添加第一个元素时，需要先初始化数组大小
        if (table == EMPTY_TABLE) {
        //数组初始化
            inflateTable(threshold);
        }
        //如果key 为 null，会将这个 entry 放在数组的第一个元素位置 table[0]
        if (key == null)
            return putForNullKey(value);
        //1）计算key的hash值
        int hash = hash(key);
        //2）根据 key 哈希值和数组长度计算存放位置的下标
        int i = indexFor(hash, table.length);
        //3）遍历对应下标处的链表，判断是否有重复的 key ，如果有直接覆盖并返回旧值
        for (Entry<K, V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }

        modCount++;
        //4）如果不存在重复的key，则将新的entry添加到链表中
        addEntry(hash, key, value, i);
        return null;
}
```
#### HashMap数组初始化【JDK1.7】

当第一个元素插入时，会初始化数组，计算数组的初始化大小及扩容阈值

```java
private void inflateTable(int toSize) {
        // 确保数组大小是2的n次方
        //比如toSize=3,计算结果为4；toSize=10，计算结果为16
        int capacity = roundUpToPowerOf2(toSize);
        //计算扩容阈值 数组大小 * 负载因子
        threshold = (int) Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);
        //初始化数组
        table = new Entry[capacity];
        initHashSeedAsNeeded(capacity);
    }
```
#### HashMap元素存放位置计算【JDK1.7】

使用key的hash值与数组长度大小减一取模

  ```java
  static int indexFor(int h, int length) {
        // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
        return h & (length-1);
    }
```
#### HashMap添加元素到链表【JDK1.7】

先判断key值是否重复，如果不重复添加新元素到链表，添加之前先判断是否需要扩容，如果需要则先扩容，重新计算hash，然后再将新元素存入对应链表的表头

```java
void addEntry(int hash, K key, V value, int bucketIndex) {
    // 如果 HashMap 大小已经达到了阈值，并且新元素要插入的数组位置已经有元素，那么此时要先扩容
    if ((size >= threshold) && (null != table[bucketIndex])) {
        // 扩容，数组大小的2倍
        resize(2 * table.length);
        // 扩容以后，重新计算 hash 值
        hash = (null != key) ? hash(key) : 0;
        // 重新计算扩容后元素存储的新下标
        bucketIndex = indexFor(hash, table.length);
    }
    // 添加元素到扩容后的链表
    createEntry(hash, key, value, bucketIndex);
}
// 新值存放到链表的表头
void createEntry(int hash, K key, V value, int bucketIndex) {
    Entry<K,V> e = table[bucketIndex];
    table[bucketIndex] = new Entry<>(hash, key, value, e);
    size++;
}
```
#### HashMap数组扩容【JDK1.7】

在插入新值的时候，如果当前的 size 已经达到了阈值，并且要插入的数组位置已经有元素，就会触发扩容，扩容后，数组大小为原来的 2 倍。
扩容就是用一个新的大数组替换原来的小数组，并将原来数组中的值迁移到新的数组中

```java
void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        //如果原数组大小等于最大容量值，则设置容量阈值为整形（Integer）最大值
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }
        //创建新数组
        Entry[] newTable = new Entry[newCapacity];
        //将原数组中的值迁移到扩容后的新数组中
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        table = newTable;
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
    }
```
### JDK1.7取值（get）分析

相对于存值（put），HashMap**取值**相对简单，大致逻辑为：

🌂根据key计算hash

🌂使用key的hash值与数组长度取模（length-1）得到元素存储位置下标

🌂遍历数组中在该下标处的链表，直到找到相等的key

```java
 public V get(Object key) {
        //如果key为null，获取对应值
        if (key == null)
            return getForNullKey();
        //如果key不为null，计算hash值、计算位置下标、遍历链表
        Entry<K,V> entry = getEntry(key);

        return null == entry ? null : entry.getValue();
    }
    //获取key值为null的对应值
    private V getForNullKey() {
        if (size == 0) {
            return null;
        }
        //遍历数组第一个位置的链表，找到key为null的值
        for (Entry<K,V> e = table[0]; e != null; e = e.next) {
            if (e.key == null)
                return e.value;
        }
        return null;
    }

final Entry<K,V> getEntry(Object key) {
        if (size == 0) {
            return null;
        }
        //计算key的hash
        int hash = (key == null) ? 0 : hash(key);
        //计算存储位置下标并遍历对应位置链表，直到找到key相等的值
        for (Entry<K,V> e = table[indexFor(hash, table.length)];
             e != null;
             e = e.next) {
            Object k;
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                return e;
        }
        return null;
    }
```
### HashMap存值（put）分析【JDK1.8】

#### HashMap存值（put）源码走读【JDK1.8】

相对于jdk1.7中HashMap存值，jdk1.8的逻辑相对复杂，因为需要判断数据节点类型是链表还是红黑树，然后使用对应的方法进行查找

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
 
// onlyIfAbsent 如果是 true，只有在不存在该 key 时才会进行 put 操作
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 第一次 put 值的时候，会触发下面的 resize()，类似 java7 的第一次 put 要初始化数组长度
    
    //第一次 resize 和后续的扩容有些不同，
    //首次扩容触发条件是，数组为null或数组长度为0
    //首次扩容使用数组初始化默认值，容量为16
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
        
    
    //首先计算数组下标，然后判断该位置是否为null，如果为null，实例化一个新node并存入该位置
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
 
    else {
        // 数组该位置有数据
        Node<K,V> e; K k;
        // 判断该位置的key是否相等，如果相等，则取出该位置节点
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        // 如果该节点是代表红黑树的节点，调用红黑树的插值方法
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            // 数组该位置上是一个链表
            for (int binCount = 0; ; ++binCount) {
                // 插入到链表的最后面(Java7 是插入到链表的最前面)
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    // TREEIFY_THRESHOLD 为 8，所以，如果新插入的值是链表中的第 9 个
                    // 会触发 treeifyBin，将链表转换为红黑树
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                // 如果在该链表中找到了"相等"的 key(== 或 equals)
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    // 结束循环，e 为链表中与要插入的新值的 key 相等的 node
                    break;
                p = e;
            }
        }
        // 存在旧值的key与要插入的key相等，进行值覆盖
        if (e != null) {
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    // 如果 HashMap 由于新插入的值导致 size 已经超过了阈值，需要进行扩容
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```
#### HashMap数组扩容【JDK1.8】

resize() 方法用于初始化数组或数组扩容，初始化时使用默认容量（16）和默认阈值（0.74*16），后来扩容时，容量为原来的 2 倍，并进行数据迁移

```java
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    //原有数组容量
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    //原有数组阈值
    int oldThr = threshold;
    //初始化新数组容量和阈值
    int newCap, newThr = 0;
    
    if (oldCap > 0) { 
        //如果原有数组容量超过最大容量限制，设置阈值大小为整形最大值
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        // 将数组大小扩大一倍
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            // 将阈值扩大一倍
            newThr = oldThr << 1; // double threshold
    }
    else if (oldThr > 0) 
        // 使用 new HashMap(int initialCapacity) 初始化后，首次插入值时
        newCap = oldThr;
    else {
        // 首次插入值，数组初始化大小
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
 
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
 
    // 创建新数组
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    // 如果是初始化数组，到这里就结束了，返回 newTab 即可
    table = newTab; 
 
    if (oldTab != null) {
        // 开始遍历原数组，进行数据迁移。
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                // 如果该数组位置上只有一个元素，将该元素迁移到新数组
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                // 如果是红黑树，根据红黑树规则将元素迁移到对应树节点
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { 
                    // 如果是链表，要将此链表一拆为二，放到新的数组中，并且保持先后顺序
                    // loHead、loTail 对应一条链表，hiHead、hiTail 对应另一条链表
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        // 第一条链表
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        // 第二条链表的新的位置是 j + oldCap
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

### JDK1.8取值（get）分析

取值逻辑如下：

1）计算 key 的 hash 值，根据 hash 值计算对应数组下标: hash & (length-1)

2）判断数组该位置处的key是否相等，如果不等，走第（3）步，否则结束

3）判断该元素类型是否是 TreeNode，如果是，用红黑树的方法取数据，如果不是，走第（4）步

4）遍历链表，直到找到相等(==或equals)的 key

   ```java
 public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }

    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        //计算元素存储位置下标
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            //如果key相等，返回该node节点
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;

            if ((e = first.next) != null) {
                //如果节点node是红黑树，按照红黑树查找法查找
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                //如果是链表，遍历链表，直到找到key相等的节点
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }

```
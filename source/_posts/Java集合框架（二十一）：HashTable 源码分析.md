---
title: Java集合框架（二十一）：HashTable 源码分析
date: 2020-05-01 10:30:36
tags: 框架
categories: java集合框架
---
## 1、HashTable 简介

和 HashMap 一样，Hashtable 也是一个**散列表**。从Java 1.2开始，这个类被改进以实现Map接口，使其成为Java Collections Framework的成员。 与新的集合实现不同，Hashtable是同步的。 如果不需要线程安全实现，建议使用HashMap代替Hashtable。 如果需要线程安全的高度并发实现，那么建议使用ConcurrentHashMap代替Hashtable。

该类实现了一个哈希表，它将键映射到值。 任何非null对象都可以用作键或值。要成功存储和检索哈希表中的对象，用作键的对象必须实现hashCode方法和equals方法。

通常，默认负载系数（0.75）在时间和空间成本之间提供了良好的折衷。 较高的值会减少空间开销，但会增加查找条目的时间成本（这反映在大多数Hashtable操作中，包括get和put）。

初始容量控制了浪费空间和重新运算操作的需要之间的权衡，这是非常耗时的。 如果初始容量大于Hashtable将包含的最大条目数除以其加载因子，则不会发生重复操作。 但是，将初始容量设置得太高会浪费空间。

如果要将多个元素设置为Hashtable，则创建具有足够容量的初始大小可以更有效地插入元素，而不是根据需要执行自动扩容来扩展表。

### 1.1、HashTable 定义

![](https://s1.ax1x.com/2020/04/16/JF184s.png)

```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable
```



从源码中，我们可以看出，Hashtable 继承于 Dictionary 类，实现了 Map, Cloneable, java.io.Serializable接口。

其中Dictionary类是任何可将键映射到相应值的类（如 Hashtable）的抽象父类，每个键和值都是对象。

```java
Dictionary 源码注释中有这样一句话，NOTE: This class is obsolete. New implementations should implement the Map interface, rather than extending this class.（ Dictionary 这个类过时了，新的实现类应该实现Map接口。）
```



### 1.2、HashTable 属性

Hashtable 是通过"拉链法"实现的哈希表。它包括几个重要的成员变量：table, count, threshold, loadFactor, modCount。

🌂table：是一个 Entry[] 数组类型，而 Entry（在 HashMap 中有讲解过）实际上就是一个单向链表。哈希表的"key-value键值对"都是存储在Entry数组中的。

🌂count：是 Hashtable 的大小，它是 Hashtable 保存的键值对的数量。

🌂threshold：是 Hashtable 的阈值，用于判断是否需要调整 Hashtable 的容量。threshold 的值=“容量*加载因子”。

🌂loadFactor：是加载因子。

🌂modCount：是用来实现 fail-fast 机制的。

```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {
/**
 * 保存key-value的数组。
 * Hashtable同样采用单链表解决冲突，每一个Entry本质上是一个单向链表
 */
private transient Entry<?,?>[] table;

/**
 * Hashtable中键值对的数量，这个大小并不是HashTable的容器大小，而是他所包含Entry键值对的数量。
 */
private transient int count;

/**
 *  阈值，用于判断是否需要调整Hashtable的容量（threshold = 容量*加载因子）
 *
 * @serial
 */
private int threshold;

/**
 * 加载因子
 *
 * @serial
 */
private float loadFactor;

/**
 * Hashtable被改变的次数，用于fail-fast机制的实现。
 * 所谓快速失败就是在并发集合中，其进行迭代操作时，若有其他线程对其进行结构性的修改，这时迭代器会立马感知* 到，并且立即抛出ConcurrentModificationException异常，而不是等到迭代完成之后才告诉你
 */
private transient int modCount = 0;
    
}
```



其中table中存储的是Entry节点，Entry节点直接采用内部类的方式实现，实现了Map.Entry类，其数据结构源码如下：

```java
private static class Entry<K,V> implements Map.Entry<K,V> {
    int hash;
    K key;
    V value;
    Entry<K,V> next;// 下一个节点，构成链表
}
```



Entry 节点中存储了元素的hash值，key, value，next引用。至此Hashtable的数据结构从源码就能看出来是数组+链表的方式实现的。

### 1.3、HashTable 构造方法

#### 1.3.1、Hashtable()

在默认构造方法中，调用了Hashtable(int initialCapacity, float loadFactor)方法，初始Hashtable的容量为11，负载因子为0.75，那么初始阈值就是8。这点和HashMap很不同，HashMap在初始化时，table的大小总是2的幂次方，即使给定一个不是2的幂次方容量的值，也会自动初始化为最接近其2的幂次方的容量。

 

```java
  /**
     * 使用默认初始容量（11）和加载因子（0.75）构造一个新的空哈希表。
     */
    public Hashtable() {
        this(11, 0.75f);
    }
```



#### 1.3.2、Hashtable(int initialCapacity)

使用指定的初始容量和默认加载因子（0.75）构造一个新的空哈希表。

```java
/**
     * 使用指定的初始容量和默认加载因子（0.75）构造一个新的空哈希表。
     *
     * @param     initialCapacity   初始容量
     * @exception IllegalArgumentException if the initial capacity is less
     *              than zero.
     */
    public Hashtable(int initialCapacity) {
        this(initialCapacity, 0.75f);
    }
```



#### 1.3.3、Hashtable(int initialCapacity, float loadFactor)

使用指定的初始容量和指定的加载因子构造一个新的空哈希表。

```java
/**
     * 使用指定的初始容量和指定的加载因子构造一个新的空哈希表。
     *
     * @param      initialCapacity   初始容量
     * @param      loadFactor        加载因子
     * @exception  IllegalArgumentException  if the initial capacity is less
     *             than zero, or if the load factor is nonpositive.
     */
    public Hashtable(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal Load: "+loadFactor);

        if (initialCapacity==0)
            initialCapacity = 1;
        this.loadFactor = loadFactor;
        table = new Entry<?,?>[initialCapacity];
        threshold = (int)Math.min(initialCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
    }
```



#### 1.3.4、Hashtable(Map<? extends K,? extends V> t)

构造一个新的哈希表，其具有与给定Map相同的映射。 创建哈希表的初始容量足以容纳给定Map中的映射和默认加载因子（0.75）。

```java
 /**
     * 构造一个新的哈希表，其具有与给定Map相同的映射。 创建哈希表的初始容量足以容纳给定Map中的映射和默认加载因子（0.75）。
     *
     * @param t 指定的map
     * @throws NullPointerException 如果给定的 map 是 null.
     * @since   1.2
     */
    public Hashtable(Map<? extends K, ? extends V> t) {
        this(Math.max(2*t.size(), 11), 0.75f);
        putAll(t);
    }
```



## 2、HashTable 源码分析

### 2.1、put 方法

put方法是同步的，即线程安全的，这点和HashMap不一样，还有具体的put操作和HashMap也存在很大的差别，Hashtable插入的时候是插入到链表头部，而HashMap是插入到链表尾部

put 方法的整体逻辑如下：

🌂判断 value 是否为空，为空则抛出异常；
🌂计算 key 的 hash 值，并根据 hash 值获得 key 在 table 数组中的位置 index，如果 table[index] 元素不为空，则进行迭代，如果遇到相同的 key，则直接替换，并返回旧 value；
🌂否则，我们可以将其插入到 table[index] 位置。
    

```java
public synchronized V put(K key, V value) {
        // Hashtable 中不能插入value为null的元素
        if (value == null) {
            throw new NullPointerException();
        }
   // 若“Hashtable中已存在键为key的键值对”，则用“新的value”替换“旧的value”
    Entry<?,?> tab[] = table;
    //直接取key的hashCode()作为哈希地址，这与HashMap的取hashCode()之后再进行hash()的结果作为哈希地址 不一样
    int hash = key.hashCode();
    //数组下标=(哈希地址 & 0x7FFFFFFF) % Hashtable容量，这与HashMap的数组下标=哈希地址 & (HashMap容量-1)计算数组下标方式不一样，前者是取模运算，后者是位于运算，这也是为什么HashMap的容量要是2的幂次方的原因，效率上后者的效率更高。
    int index = (hash & 0x7FFFFFFF) % tab.length;
    //获取key所在索引的entry
    @SuppressWarnings("unchecked")
    Entry<K,V> entry = (Entry<K,V>)tab[index];
    //遍历Entry链表，如果链表中存在key、哈希地址相同的节点，则将值更新，返回旧值
    for(; entry != null ; entry = entry.next) {
         //如果key已经存在
        if ((entry.hash == hash) && entry.key.equals(key)) {
            //保存旧的value
            V old = entry.value;
            //替换value
            entry.value = value;
            //返回旧的value
            return old;
        }
    }
    //如果为新的节点，则调用addEntry()方法添加新的节点
    addEntry(hash, key, value, index);
    //插入成功返回null
    return null;
}

 /**
 * 根据参数向table中添加entry
 */
private void addEntry(int hash, K key, V value, int index) {
   // 修改次数+1
    modCount++;
    // 记录当前的table
    Entry<?,?> tab[] = table;
    // 如果当前的entry数量大于临界值
    if (count >= threshold) {
         // 扩容
        rehash();
        //记录新的table
        tab = table;
        //重新计算key的hash
        hash = key.hashCode();
        //重新计算index
        index = (hash & 0x7FFFFFFF) % tab.length;
    }

    // 获取当前Entry链表的引用 复赋值给e
    @SuppressWarnings("unchecked")
    Entry<K,V> e = (Entry<K,V>) tab[index];
    //创建新的Entry链表，将新的节点插入到Entry链表的头部，再指向之前的Entry，即在链表头部插入节点
    tab[index] = new Entry<>(hash, key, value, e);
    //table大小+1
    count++;
}
```



通过源码，我们可以看出 HashMap 与 Hashtable 计算索引的方式是不一样的：

🌂HashMap 计算索引的方式是h&(length-1),而Hashtable用的是模运算，效率上是低于HashMap的
🌂Hashtable计算索引时将hash值先与上0x7FFFFFFF,这是为了保证hash值始终为正数
🌂特别需要注意的是这个方法加了synchronized关键字，也就意味着这个Hashtable是个线程安全的类，这也是它和HashMap最大的不同点

### 2.2、get 方法

get方法也是同步的，和HashMap不一样,即线程安全，具体的get操作和HashMap也有区别。

```java
 public synchronized V get(Object key) {
        Entry<?,?> tab[] = table;
        // 直接获取key的hashCode()作为哈希地址
        int hash = key.hashCode();
        // 通过(哈希地址 & 0x7FFFFFFF)与Hashtable容量做%运算 计算出下标
        int index = (hash & 0x7FFFFFFF) % tab.length;
        //遍历Entry链表，如果链表中存在key、哈希地址一样的节点，则找到 返回该节点的值，否者返回null
        for (Entry<?,?> e = tab[index] ; e != null ; e = e.next) {
            if ((e.hash == hash) && e.key.equals(key)) {
                return (V)e.value;
            }
        }
        return null;
    }
```



### 2.3、remove 方法

```java
public synchronized V remove(Object key) {
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Entry<K,V> e = (Entry<K,V>)tab[index];
        // 遍历Entry链表，e为当前节点，prev为上一个节点
        for(Entry<K,V> prev = null ; e != null ; prev = e, e = e.next) {
           // 找到key及哈希地址相等的节点
            if ((e.hash == hash) && e.key.equals(key)) {
                modCount++;
                // 如果上一个节点不为空，将上一个节点的next指向当前节点的next，即将当前节点移除链表
                if (prev != null) {
                    prev.next = e.next;
                } else {
                // 如果上一个节点为空，即当前节点为头结点，将table数组保存的链表头结点地址改成当前节点的下一个节点
                    tab[index] = e.next;
                }
                // 键值对数量-1
                count--;
                // 获取被删除节点的值，并返回
                V oldValue = e.value;
                e.value = null;
                return oldValue;
            }
        }
        return null;
    }
```



### 2.4、rehash 方法

Hashtable 的rehash方法是用来扩容哈希表的。Hashtable每次扩容,容量都为原来的2倍加1,而HashMap为原来的2倍。

```java
protected void rehash() {
    // 获取旧的Hashtable的容量
    int oldCapacity = table.length;
    // 获取旧的Hashtable引用
    Entry<?,?>[] oldMap = table;

    // 新的Hashtable容量=旧的Hashtable容量 * 2 + 1，HashMap是新的Hashtable容量=旧的Hashtable容量 * 2。
    int newCapacity = (oldCapacity << 1) + 1;
    // 如果新的Hashtable容量大于允许的最大容量值(Integer的最大值 - 8)
    if (newCapacity - MAX_ARRAY_SIZE > 0) {
        // 如果旧的容量等于允许的最大容量值则返回
        if (oldCapacity == MAX_ARRAY_SIZE)
            // Keep running with MAX_ARRAY_SIZE buckets
            return;
        // 新的容量等于允许的最大容量值
        newCapacity = MAX_ARRAY_SIZE;
    }
    // 使用新的容量构建一个新的Hashtable 
    Entry<?,?>[] newMap = new Entry<?,?>[newCapacity];

    modCount++;
    // 计算新的阈值
    threshold = (int)Math.min(newCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
    table = newMap;
    // 扩容后迁移Hashtable的Entry链表
    for (int i = oldCapacity ; i-- > 0 ;) {
        for (Entry<K,V> old = (Entry<K,V>)oldMap[i] ; old != null ; ) {
            Entry<K,V> e = old;
            old = old.next;

            int index = (e.hash & 0x7FFFFFFF) % newCapacity;
            e.next = (Entry<K,V>)newMap[index];
            newMap[index] = e;
        }
    }
}
```



## 3、Hashtable 如何保证线程安全

通过Hashtable源码，我们发现对外提供的public方法，几乎全部加上了synchronized关键字。

```java
public synchronized int size(){}
public synchronized boolean isEmpty() {}
public synchronized Enumeration<K> keys() {}
public synchronized Enumeration<V> elements() {}
public synchronized boolean contains(Object value) {}
public synchronized boolean containsKey(Object key) {}
public synchronized V get(Object key) {}
public synchronized V put(K key, V value) {}
public synchronized V remove(Object key) {}
public synchronized void putAll(Map<? extends K, ? extends V> t) {}
public synchronized void clear() {}
public synchronized Object clone() {}
public synchronized String toString() {}
public synchronized boolean equals(Object o) {}
public synchronized int hashCode() {}
```



所以从这个特性上看，Hashtable是通过简单粗暴的方式来保证线程安全的额。所以Hashtable的性能在多线程环境下会非常低效。前面介绍的ConcurrentHashMap其实就是Java开发团队为了替换他而开发的，性能提高了不少。

所以建议大家在多线程环境下使用 ConcurrentHashMap，或者用HashMap配合外部锁，例如ReentrantLock来提高效率。

## 4、Hashtable 与 HashMap 的区别

**不同点：**

🌂HashMap 继承于 AbstractMap抽象类，Hashtable 继承于Dictionay抽象类
🌂HashMap 是非线程安全的，Hashtable是线程安全的，所以Hashtable效率比较低
🌂HashMap 通过key的hashCode()进行hash()得到哈希地址，数组下标=哈希地址 & (容量 - 1)，采用的是与运算，所以容量需要是2的幂次方结果才和取模运算结果一样。而Hashtable则是：数组下标=(key的🌂hashCode() & 0x7FFFFFFF ) % 容量，采用的取模运算，所以容量没要求
🌂HashMap 允许键值为null，而Hashtable不允许键值为null
🌂HashMap 的容量扩容按照原来的容量2，而Hashtable的容量扩容按照原来的容量2+1
🌂HashMap 的容量默认值为16，而Hashtable的默认值是11
🌂HashMap 是将节点插入到链表的尾部，而Hashtable是将节点插入到链表的头部
🌂HashMap 采用了数组+链表+红黑树，而Hashtable采用数组+链表

**相同点：**

🌂HashMap 和 Hashtable 都实现了Map接口
🌂HashMap 和 Hashtable 的负载因子默认都是0.75
🌂都是采用链地址法即拉链法处理哈希冲突
🌂相同哈希地址可能分配到不同的链表，同一个链表内节点的哈希地址不一定相同：因为HashMap和Hashtable都会扩容，扩容后相同的哈希地址取到的数组下标也就不一样。


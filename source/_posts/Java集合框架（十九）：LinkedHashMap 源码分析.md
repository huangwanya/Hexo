---
title: Java集合框架（十九）：LinkedHashMap 源码分析
date: 2020-04-16 10:02:30
tags: 框架
categories: java集合框架
---
## 1、LinkedHashMap 简介

**HashMap **是**无序**的，HashMap 在 put 的时候是根据 key 的 hashcode 进行 hash 然后放入对应的位置。所以在按照一定顺序 put 进 HashMap 中后，再次遍历出 HashMap 的顺序跟 put 的顺序不同（除非在 put 的时候 key 已经按照 hashcode 排好序了）。

JAVA 在 JDK1.4 以后提供了 LinkedHashMap 来帮助我们实现了有序的 HashMap。

LinkedHashMap** 继承**自 HashMap，是Map接口的哈希表和链接列表实现，具有**可预知的迭代顺序**。此实现提供所有可选的映射操作，并允许使用 **null** 值和 **null** 键。

LinkedHashMap 实现与 HashMap 的不同之处在于，LinkedHashMap 在 HashMap 基础上，维护了一条**双向链表**。此链接列表定义了迭代顺序，该迭代顺序可以是插入顺序或者是访问顺序。

除此之外，LinkedHashMap 对访问顺序也提供了相关支持。在一些场景下，该特性很有用，比如**缓存**。

**注意**，此**实现不是同步**的。如果多个线程同时访问链接的哈希映射，而其中至少一个线程从结构上修改了该映射，则它必须保持外部同步。

根据链表中元素的顺序可以分为：按插入顺序的链表，和按访问顺序(调用 get 方法)的链表。默认是按插入顺序排序，如果指定按访问顺序排序，那么调用get方法后，会将这次访问的元素移至链表尾部，不断访问可以形成按访问顺序排序的链表。

在实现上，LinkedHashMap 它继承自 **HashMap(public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V>)**、底层使用哈希表与双向链表来保存所有元素。其基本操作与父类 HashMap 相似，它通过重写父类相关的方法，来实现自己的链接列表特性。

所以，在学习 LinkedHashMap 的源码之前，很有必要先看懂 HashMap 的源码。关于 HashMap 的源码分析，大家可以参考笔者之前的一篇文章[**Java集合框架（十八）：HashMap 源码分析**](http://ur868q.coding-pages.com/2020/04/12/Java%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%EF%BC%88%E5%8D%81%E5%85%AB%EF%BC%89%EF%BC%9AHashMap%20%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/)


## 2、LinkedHashMap 数据结构

![](https://s1.ax1x.com/2020/04/16/JFeoZR.png)
图片来自网络

LinkedHashMap 和 HashMap 的区别在于它们的基本数据结构上，LinkedHashMap 中采用的链表是双向循环链表，也就是Entry，这种数据结构，最关键的是保证在增加节点、删除节点时不断链。

### 2.1、Entry 的继承体系

![](https://s1.ax1x.com/2020/04/16/JFe4sJ.png)

LinkedHashMap 内部类 Entry 继承自 HashMap 内部类 Node，并新增了两个引用，分别是 before 和 after。这两个引用的用途就是用于维护双向链表。

TreeNode 继承 LinkedHashMap 的内部类 Entry 后，就具备了和其他 Entry 一起组成链表的能力。

但是这里需要大家考虑一个问题。当我们使用 HashMap 时，TreeNode 并不需要具备组成链表能力。如果继承 LinkedHashMap 内部类 Entry ，TreeNode 就多了两个用不到的引用，这样做不是会浪费空间吗？

首先这么做确实会浪费空间，但与 TreeNode 通过继承获取的组成链表的能力相比，这点浪费是值得的。在 HashMap 的设计思路注释中，有这样一段话，大致意思如下：

```java
Because TreeNodes are about twice the size of regular nodes, we
use them only when bins contain enough nodes to warrant use
(see TREEIFY_THRESHOLD). And when they become too small (due to
removal or resizing) they are converted back to plain bins. In
usages with well-distributed user hashCodes, tree bins are
rarely used.
```
TreeNode 对象的大小约是普通 Node 对象的2倍，我们仅在桶（bin）中包含足够多的节点时再使用。当桶中的节点数量变少时（取决于删除和扩容），TreeNode 会被转成 Node。当用户实现的 hashCode 方法具有良好分布性时，树类型的桶将会很少被使用。

一般情况下，只要 hashCode 的实现够合理，Node 组成的链表很少会被转成由 TreeNode 组成的红黑树。也就是说 TreeNode 使用的并不多，浪费那点空间是可接受的。假如 TreeNode 的机制继承自 Node 类，那么它要想具备组成链表的能力，就需要 Node 去继承 LinkedHashMap 的内部类 Entry。这样就得不偿失了，浪费很多空间去获取不一定用得到的能力。


## 3、LinkedHashMap 源码分析

### 3.1、LinkedHashMap 类图

```java
public class LinkedHashMap<K,V>
    extends HashMap<K,V>
    implements Map<K,V>
```

![](https://s1.ax1x.com/2020/04/16/JFe5L9.png)

### 3.2、LinkedHashMap 构造函数

LinkedHashMap有5个构造方法，从构造方法中可以看出，默认都采用插入顺序对key进行排序来维持取出键值对的顺序。所有构造方法都是通过调用父类的构造方法来创建对象的。

   ```java
 /**
     * 排序模式，true-访问顺序，false-插入顺序
     */
    final boolean accessOrder;
   /**
     * 使用指定的初始容量，加载因子和排序模式构造一个空的LinkedHashMap实例。
     *
     * @param  initialCapacity 初始容量
     * @param  loadFactor      加载因子
     * @throws IllegalArgumentException 如果初始容量为负或负载因子是非正的
     */
    public LinkedHashMap(int initialCapacity, float loadFactor) {
        super(initialCapacity, loadFactor);
        //按插入顺序对key排序
        accessOrder = false;
    }

    /**
     * 使用指定的初始容量和默认加载因子（0.75）构造一个空的按插入顺序排序的LinkedHashMap实例。
     *
     * @param  初始容量
     * @throws IllegalArgumentException 如果初始容量为负
     */
    public LinkedHashMap(int initialCapacity) {
        super(initialCapacity);
        //按插入顺序对key排序
        accessOrder = false;
    }

    /**
     * 使用默认初始容量（16）和加载因子（0.75）构造一个空的按插入顺序排序的 LinkedHashMap 实例。
     */
    public LinkedHashMap() {
        super();
        //按插入顺序对key排序
        accessOrder = false;
    }

    /**
     * 使用与指定映射相同的映射构造一个按插入顺序排序的LinkedHashMap实例。 LinkedHashMap 实例使用默认加载因子（0.75）和足以保存指定映射中的映射的初始容量创建。
     *
     * @param  m 映射的映射将放置在此映射中
     * @throws NullPointerException 如果给定的 map 为null
     */
    public LinkedHashMap(Map<? extends K, ? extends V> m) {
        super();
        //按插入顺序对key排序
        accessOrder = false;
        putMapEntries(m, false);
    }

    /**
     * 使用指定的初始容量，加载因子和排序模式构造一个空的LinkedHashMap实例。
     *
     * @param  initialCapacity 初始容量
     * @param  loadFactor      加载因子
     * @param  accessOrder     排序模式
     * @throws IllegalArgumentException 如果初始容量为负或负载因子是非正的
     */
    public LinkedHashMap(int initialCapacity,
                         float loadFactor,
                         boolean accessOrder) {
        super(initialCapacity, loadFactor);
        this.accessOrder = accessOrder;
    }
   ```

### 3.3、LinkedHashMap 成员变量

LinkedHashMap 采用的 hash 算法和 HashMap 相同，但是它重新定义了数组中保存的元素 Entry，该 Entry 除了保存当前对象的引用外，还保存了其上一个元素 before 和下一个元素 after 的引用，从而在哈希表的基础上又构成了双向链接列表。

 ```java
/**
     * 双向链表的表头元素。
     */
    transient LinkedHashMap.Entry<K,V> head;

    /**
     * 双向链表的表尾元素。
     */
    transient LinkedHashMap.Entry<K,V> tail;

    /**
     * 如果为true，则按照访问顺序；如果为false，则按照插入顺序。
     *
     * @serial
     */
    final boolean accessOrder;
 ```

## 4、常用方法源码分析

### 4.1、containsValue( Object value)

  ```java
  /**
     * 如果此Map存在与指定值匹配的一个或多个键，则返回true。
     *
     * @param value 参数value
     * @return 如果linkedHashMap中的键值对存在与指定值匹配的一个或多个键，返回true
     */
    public boolean containsValue(Object value) {
        //遍历双向循环链表
        for (LinkedHashMap.Entry<K,V> e = head; e != null; e = e.after) {
            V v = e.value;
            if (v == value || (value != null && value.equals(v)))
                return true;
        }
        //否则返回false
        return false;
    }
  ```

### 4.2、get( Object key)

  ```java
 /**
     * 返回指定键映射到的值，或者如果此映射不包含键的映射，则返回null。
     *
     * 返回值null不一定表示不包含键的映射，也可能该键的映射值null。 containsKey操作可用于区分这两种情况。
     */
    public V get(Object key) {
        Node<K,V> e;
        if ((e = getNode(hash(key), key)) == null)
            return null;
        if (accessOrder)
            afterNodeAccess(e);
        return e.value;
    }
  ```

### 4.3、getOrDefault( Object key, V defaultValue)

```java
    /**
     * 返回指定键映射到的值，如果不包含键的映射，则返回defaultValue。
     */
    public V getOrDefault(Object key, V defaultValue) {
       Node<K,V> e;
       if ((e = getNode(hash(key), key)) == null)
           return defaultValue;
       if (accessOrder)
           afterNodeAccess(e);
       return e.value;
   }
```

### 4.4、clear()

  ```java
  /**
     * 清空linkedHashMap
     */
    public void clear() {
        //清空哈希表
        super.clear();
        //清空双向循环链表
        head = tail = null;
    }
  ```

### 4.5、removeEldestEntry( Map.Entry<K,V> eldest)

  ```java
/**
     * 如果此映射应该删除最旧条目，则返回true。
     * 在将新条目插入后，put和putAll将调用此方法。 它为实现者提供了在每次添加新条目时删除最旧条目的机会。
     * 如果应用于缓存，这将非常有用：它允许映射通过删除过时条目来减少内存消耗。
     * 
     * 示例：此方法允许LinkedHashMap 存储100个条目，然后在每次添加新条目时删除最旧的条目，始终保持100个条目的稳定状态。
     *  private static final int MAX_ENTRIES = 100;
     *  protected boolean removeEldestEntry(Map.Entry eldest) {
     *   return size() > MAX_ENTRIES;
     *  }
     *
     *  这个方法一般不会直接修改map，而是通过返回true或者false来控制是否修改map。
     *  这个方法仅仅返回false，这样头节点就永远都不会被删除了。
     * @param    eldest 头节点
     * @return   如果map应该删除头节点就返回true，否则返回false
     */
    protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
        return false;
    }
  ```

### 4.6、keySet()

  ```java
/**
     * 返回 linkedHashMap中所有key的集合
     * 改变linkedHashMap会影响到set，反之亦然。
     * 如果当迭代器迭代set时，linkedHashMap被修改(除非是迭代器自己的remove()方法)，迭代器的结果是不确定的。
     * set支持元素的删除，通过Iterator.remove、Set.remove、removeAll、retainAll、clear操作删除hashMap中对应的键值对。不支持add和addAll方法。
     *
     * @return 返回linkedHashMap中所有key的set视图
     */
    public Set<K> keySet() {
        Set<K> ks = keySet;
        if (ks == null) {
            ks = new LinkedKeySet();
            keySet = ks;
        }
        return ks;
    }
    
    final class LinkedKeySet extends AbstractSet<K> {
        public final int size()                 { return size; }
        public final void clear()               { LinkedHashMap.this.clear(); }
        public final Iterator<K> iterator() {
            return new LinkedKeyIterator();
        }
        public final boolean contains(Object o) { return containsKey(o); }
        public final boolean remove(Object key) {
            return removeNode(hash(key), key, null, false, true) != null;
        }
        public final Spliterator<K> spliterator()  {
            return Spliterators.spliterator(this, Spliterator.SIZED |
                                            Spliterator.ORDERED |
                                            Spliterator.DISTINCT);
        }
        public final void forEach(Consumer<? super K> action) {
            if (action == null)
                throw new NullPointerException();
            int mc = modCount;
            for (LinkedHashMap.Entry<K,V> e = head; e != null; e = e.after)
                action.accept(e.key);
            if (modCount != mc)
                throw new ConcurrentModificationException();
        }
    }
  ```

### 4.7、values()

```java
  /**
     * 返回linkedHashMap中所有value的collection集合
     * 改变linkedHashMap会改变collection，反之亦然。
     * 如果当迭代器迭代collection时，linkedHashMap被修改（除非是迭代器自己的remove()方法），迭代器的结果是不确定的。
     * collection支持元素的删除，通过Iterator.remove、Collection.remove、removeAll、retainAll、clear操作删除linkedHashMap中对应的键值对。不支持add和addAll方法。
     *
     * @return 返回linkedHashMap中所有value的collection集合
     */
    public Collection<V> values() {
        Collection<V> vs = values;
        if (vs == null) {
            vs = new LinkedValues();
            values = vs;
        }
        return vs;
    }

    final class LinkedValues extends AbstractCollection<V> {
        public final int size()                 { return size; }
        public final void clear()               { LinkedHashMap.this.clear(); }
        public final Iterator<V> iterator() {
            return new LinkedValueIterator();
        }
        public final boolean contains(Object o) { return containsValue(o); }
        public final Spliterator<V> spliterator() {
            return Spliterators.spliterator(this, Spliterator.SIZED |
                                            Spliterator.ORDERED);
        }
        public final void forEach(Consumer<? super V> action) {
            if (action == null)
                throw new NullPointerException();
            int mc = modCount;
            for (LinkedHashMap.Entry<K,V> e = head; e != null; e = e.after)
                action.accept(e.value);
            if (modCount != mc)
                throw new ConcurrentModificationException();
        }
    }
```

### 4.8、entrySet()

```java
 /**
     * 返回此映射中包含的映射（Entry）的Set视图。 当修改映射时会影响Set视图，反之亦然。
     * 如果在对集合进行迭代时修改了映射（除非通过迭代器自己的remove操作，或者通过迭代器返回的映射条目上的set* Value操作），迭代的结果是未定义的。
     * 该集支持元素删除，它通过Iterator.remove，Set.remove，removeAll，retainAll和clear操作从映射中删除相应的映射。 它不支持add或addAll操作。 它的Spliterator通常提供更快的顺序性能，但并行性能比HashMap差。
     */
    public Set<Map.Entry<K,V>> entrySet() {
        Set<Map.Entry<K,V>> es;
        return (es = entrySet) == null ? (entrySet = new LinkedEntrySet()) : es;
    }

    final class LinkedEntrySet extends AbstractSet<Map.Entry<K,V>> {
        public final int size()                 { return size; }
        public final void clear()               { LinkedHashMap.this.clear(); }
        public final Iterator<Map.Entry<K,V>> iterator() {
            return new LinkedEntryIterator();
        }
        public final boolean contains(Object o) {
            if (!(o instanceof Map.Entry))
                return false;
            Map.Entry<?,?> e = (Map.Entry<?,?>) o;
            Object key = e.getKey();
            Node<K,V> candidate = getNode(hash(key), key);
            return candidate != null && candidate.equals(e);
        }
        public final boolean remove(Object o) {
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>) o;
                Object key = e.getKey();
                Object value = e.getValue();
                return removeNode(hash(key), key, value, true, true) != null;
            }
            return false;
        }
        public final Spliterator<Map.Entry<K,V>> spliterator() {
            return Spliterators.spliterator(this, Spliterator.SIZED |
                                            Spliterator.ORDERED |
                                            Spliterator.DISTINCT);
        }
        public final void forEach(Consumer<? super Map.Entry<K,V>> action) {
            if (action == null)
                throw new NullPointerException();
            int mc = modCount;
            for (LinkedHashMap.Entry<K,V> e = head; e != null; e = e.after)
                action.accept(e);
            if (modCount != mc)
                throw new ConcurrentModificationException();
        }
    }
```

### 4.9、链表节点的创建

链表的创建过程是在插入键值对节点时开始的，初始情况下，让 LinkedHashMap 的 head 和 tail 引用同时指向新节点，链表就算建立起来了。随后不断有新节点插入，通过将新节点接在 tail 引用指向节点的后面，即可实现链表的更新。

Map 类型的集合类是通过 put(K,V) 方法插入键值对，LinkedHashMap 本身并没有覆写父类的 put 方法，而是直接使用了父类的实现。但在 HashMap 中，put 方法插入的是 HashMap 内部类 Node 类型的节点，该类型的节点并不具备与 LinkedHashMap 内部类 Entry 及其子类型节点组成链表的能力。

在 HashMap 的 putval 方法里面创建新节点是使用 newNode 方法的，自然 LinkedHashMap 只需要重写下 newNode 方法即可。

```java
// HashMap 中实现
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

// HashMap 中实现
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0) {...}
    // 通过节点 hash 定位节点所在的桶位置，并检测桶中是否包含节点引用
    if ((p = tab[i = (n - 1) & hash]) == null) {...}
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode) {...}
        else {
            // 遍历链表，并统计链表长度
            for (int binCount = 0; ; ++binCount) {
                // 未在单链表中找到要插入的节点，将新节点接在单链表的后面
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) {...}
                    break;
                }
                // 插入的节点已经存在于单链表中
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null) {...}
            afterNodeAccess(e);    // 回调方法
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold) {...}
    afterNodeInsertion(evict);    // 回调方法
    return null;
}

// HashMap 中实现
Node<K,V> newNode(int hash, K key, V value, Node<K,V> next) {
    return new Node<>(hash, key, value, next);
}

// LinkedHashMap 中覆写
Node<K,V> newNode(int hash, K key, V value, Node<K,V> e) {
    LinkedHashMap.Entry<K,V> p =
        new LinkedHashMap.Entry<K,V>(hash, key, value, e);
    // 将 Entry 接在双向链表的尾部
    linkNodeLast(p);
    return p;
}
```

链表节点创建，主要分为**三步**：
🌂创建 p 节点
🌂关联节点
🌂返回节点

创建 p 节点没什么复杂的操作，和 HashMap 里面的一样，主要的操作在关联节点 linkNodeLast 中：

```java
// LinkedHashMap 中实现，把节点关联到链表尾部
private void linkNodeLast(LinkedHashMapEntry<K,V> p) {
    // 拿到尾节点赋值给临时变量 last
    LinkedHashMapEntry<K,V> last = tail;
    // 把创建的节点赋值给尾节点。
    tail = p;
    // 如果之前的尾节点为空，说明链表为空，头节点也赋值为 p
    if (last == null)
        head = p;
    // 不为空，说明有值，新节点的 before 指向以前的尾节点
    // 以前的尾节点的 after 指向新节点
    else {
        p.before = last;
        last.after = p;
    }
}
```

可以看到，在 linkNodeLast() 函数的内部给节点的 before 和 after属性赋值，也就把节点给串联了起来。
所以 LinkedHashMap 保证插入顺序的关键就是在 newNode 方法里面。

在 HashMap 的 putVal 方法里面发现了这个方法 afterNodeInsertion：

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}

```

这里的 afterNodeInsertion 在 HashMap 里面是空方法，专门为 LinkedHashMap 准备的，与之类似的还有两个方法：

```java
// 在新增、替换、查找的时候，如果 accessOrder 为 true 会执行。
void afterNodeAccess(Node<K,V> p) { }
// 新增元素后会在 LinkedHashMap 中调用
void afterNodeInsertion(boolean evict) { }
// 删除元素后会在 LinkedHashMap 中调用
void afterNodeRemoval(Node<K,V> p) { }
```

#### 4.9.1、afterNodeInsertion 方法

```java
void afterNodeInsertion(boolean evict) {
    LinkedHashMapEntry<K,V> first;
    //如果头节点不为 null，根据removeEldestEntry返回值判断是否移除最近最少被访问的节点
    if (evict && (first = head) != null && removeEldestEntry(first)) {
        K key = first.key;
        //HashMap 中的方法
        removeNode(hash(key), key, null, false, true);
    }
}

// 移除最近最少被访问条件之一，通过覆盖此方法可实现不同策略的缓存
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
    return false;
}
```

### 4.10、链表节点的删除

与插入操作一样，LinkedHashMap 删除操作相关的代码也是直接用父类的实现。在删除节点时，父类的删除逻辑并不会修复 LinkedHashMap 所维护的双向链表，这不是它的职责。那么删除节点后，被删除的节点该如何从双链表中移除呢？上文最后提到 HashMap 中三个回调方法运行 LinkedHashMap 对一些操作做出响应。所以，在删除及节点后，回调方法 afterNodeRemoval 会被调用。LinkedHashMap 覆写该方法，并在该方法中完成了移除被删除节点的操作。

```java
// HashMap 中实现
public V remove(Object key) {
    Node<K,V> e;
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value;
}

// HashMap 中实现
final Node<K,V> removeNode(int hash, Object key, Object value,
                           boolean matchValue, boolean movable) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (p = tab[index = (n - 1) & hash]) != null) {
        Node<K,V> node = null, e; K k; V v;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;
        else if ((e = p.next) != null) {
            if (p instanceof TreeNode) {...}
            else {
                // 遍历单链表，寻找要删除的节点，并赋值给 node 变量
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                         (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;
                } while ((e = e.next) != null);
            }
        }
        if (node != null && (!matchValue || (v = node.value) == value ||
                             (value != null && value.equals(v)))) {
            if (node instanceof TreeNode) {...}
            // 将要删除的节点从单链表中移除
            else if (node == p)
                tab[index] = node.next;
            else
                p.next = node.next;
            ++modCount;
            --size;
            afterNodeRemoval(node);    // 调用删除回调方法进行后续操作
            return node;
        }
    }
    return null;
}
```

#### 4.10.1、afterNodeRemoval 方法

```java
// LinkedHashMap 中覆写，HashMap 的 remove 方法中调用，传入删除的节点
void afterNodeRemoval(Node<K,V> e) { // unlink
    // 把删除节点 e 赋值给 p 
    // b 删除节点的前置节点
    // a 删除节点的后置节点
    LinkedHashMapEntry<K,V> p =
        (LinkedHashMapEntry<K,V>)e, b = p.before, a = p.after;
    // 首先将删除节点的引用置为 null
    p.before = p.after = null;
    // 如果删除节点前置节点是空，说明是头节点，删除后后置节点设置为头节点
    if (b == null)
        head = a;
    // 如果删除节点前置节点不是空，将删除节点的前置节点的 after 指向 删除节点的后置节点    
    else
        b.after = a;
    // 如果是尾节点，则将删除节点的前一个指向节点置为 尾节点
    if (a == null)
        tail = b;
    // 如果是尾节点，将删除节点的后置节点的前指向节点指向 删除节点的前置节点
    else
        a.before = b;
}
```

删除的过程并不复杂，主要分为**三步**：

🌂根据 hash 定位到桶位置
🌂遍历链表或调用红黑树相关的删除方法
🌂从 LinkedHashMap 维护的双链表中移除要删除的节点

### 4.11、获取数据

LinkedHashMap 重写了 get 方法，首先通过 getNode 查找节点，但是在 LinkedHashMap 方法中并没有这个 getNode 方法，是调用的 HashMap 的 getNode 方法。
需要注意的是，在调用 getNode 方法以后，如果 accessOrder 为 true ，会接着调用 afterNodeAccess 方法，这个方法是在 HashMap 中定义，在 LinkedHashMap 中实现的：

```java
// LinkedHashMap 中覆写
public V get(Object key) {
    Node<K,V> e;
    if ((e = getNode(hash(key), key)) == null)
        return null;
    // 如果 accessOrder 为 true，则调用 afterNodeAccess 将被访问节点移动到链表最后
    if (accessOrder)
        afterNodeAccess(e);
    return e.value;
}
```

#### 4.11.1、afterNodeAccess 方法

把当前操作的节点移到最后，作为尾节点。
这样的话，就会改变我们插入时候的元素的顺序，也就实现了按照访问和插入实现元素的排序。

```java
void afterNodeAccess(Node<K,V> e) { // move node to last
    // 最后一个节点
    LinkedHashMapEntry<K,V> last;
    // 如果 accessOrder 为 true，并且 尾节点不是 e，进入 if 代码块
    if (accessOrder && (last = tail) != e) {
        // 把删除节点 e 赋值给 p 
        // b 删除节点的前置节点
        // a 删除节点的后置节点
        LinkedHashMapEntry<K,V> p =
            (LinkedHashMapEntry<K,V>)e, b = p.before, a = p.after;
        // 将当前节点的后置节点引用置为 null，因为要把 当前节点作为尾节点，尾节点的后置节点为 null
        p.after = null;
        // 如果当前节点是头节点，将 a 作为头节点。
        if (b == null)
            head = a;
        // 将当前节点的前置和后置节点关联
        else
            b.after = a;
        // 如果 当前节点的后置节点不为尾节点，将当前节点的后置节点和前置节点关联   
        if (a != null)
            a.before = b;
        //当前节点是尾节点 ，将 b 作为尾节点
        else
            last = b;
        // 如果尾节点为 null，头节点也设置为当前节点 ，因为链表就一个节点   
        if (last == null)
            head = p;
        //否则 更新当前节点p的前置节点为 原尾节点last， last的后置节点是p
        else {
            p.before = last;
            last.after = p;
        }
        // 将当前节点设置为尾节点，
        tail = p;
        ++modCount;
    }
}
```

### 4.12、访问顺序的维护(afterNodeAccess 方法)

默认情况下，LinkedHashMap 是按插入顺序维护链表。不过我们可以在初始化 LinkedHashMap，指定 accessOrder 参数为 true，即可让它按访问顺序维护链表。访问顺序的原理并不复杂，当我们调用get/getOrDefault/replace等方法时，只需要将这些方法访问的节点移动到链表的尾部即可。

## 5、LinkedHashMap 总结
🌂LinkedHashMap 允许key 为 null ，value 为 null
🌂LinkedHashMap key 不可以重复，value 可以重复
🌂LinkedHashMap 内部是以数组+双向链表实现的，JDK8 以后加入了红黑树
🌂LinkedHashMap 内部键值对的存储是有序的（需要注意初始化的时候 accessOrder 属性的设置）。
🌂LinkedHashMap accessOrder 为 true，那么内部元素的顺序会根据最近访问方式排序，如果为 
🌂false，就会按照元素插入的顺序排序
🌂LinkedHashMap 初始容量为 16，负载因子为 0.75，也就是当容量达到 容量*负载因子 的时候会扩容，一次扩容增加一倍。
🌂LinkedHashMap 内部的键值对要重写 key 对象重写 hashCode 方法、equals 方法。
🌂LinkedHashMap 线程不安全的，如果想要线程安全，使用SynchronizedMap 使用 Map<String, String> map = Collections.synchronizedMap(new LinkedHashMap<String, String>(16,0.75f,true)); 来获得一个线程安全的 LinkedHashMap。SynchronizedMap 内部也是使用的 Synchronized 实现线程安全的

## 6、怎样利用LinkedHashMap实现LRU缓存？

我们只要把accessOrder设置为true，重写 removeEldestEntry(eldest) 即可。我们在 removeEldestEntry(eldest) 指定 map里面的元素数量超过指定的大小，开始删除最近最少使用的元素，为后续新增的元素准备空间。

```java
public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private static final long serialVersionUID = 1L;
    protected int maxElements;

    public LRUCache(int maxSize) {
        super(maxSize, 0.75F, true);
        this.maxElements = maxSize;
    }
    /**
     * 判断节点数是否超出限制
     * @param eldest 头节点
     * @return 超限返回 true，否则返回 false
     */
    protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
        return this.size() > this.maxElements;
    }

    public V save(K key, V val) {
        return put(key, val);
    }

    public V getOne(K key) {
        return get(key);
    }

    public boolean exists(K key) {
        return containsKey(key);
    }
}
```

**示例：**

```java
 public static void main(String[] args) {
        LRUCache<Integer, Integer> cache = new LRUCache<>(3);

        for (int i = 0; i < 5; i++) {
            cache.save(i, i * i);
        }

        System.out.println("插入5个键值对后，缓存内容：");
        System.out.println(cache + "\n");

        System.out.println("访问键值为2的节点后，缓存内容：");
        cache.getOne(7);
        System.out.println(cache + "\n");

        System.out.println("插入键值为8的键值对后，缓存内容：");
        cache.save(8, 8);
        System.out.println(cache);
    }
```

**输出：**

```java
插入5个键值对后，缓存内容：
{2=4, 3=9, 4=16}

访问键值为2的节点后，缓存内容：
{2=4, 3=9, 4=16}

插入键值为8的键值对后，缓存内容：
{3=9, 4=16, 8=8}
```

## 7、HashMap 与 LinkedHashMap 区别

**相同点：**
🌂都是基于哈希表的实现。
🌂存储的都是键值对映射。
🌂都继承了AbstractMap，实现了Map、Cloneable、Serializable。
🌂它们的构造函数都一样。
🌂默认的容量大小是16，默认的加载因子是0.75。
🌂都允许key和value为null。
🌂都是线程不安全的。

**不同点：**

| 不同点   | HashMap          | LinkedHashMap                 |
| -------- | ---------------- | ----------------------------- |
| 数据结构 | 数组+链表+红黑树 | 数组+链表+红黑树+双向循环链表 |
| 是否有序 | 无序             | 有序                          |
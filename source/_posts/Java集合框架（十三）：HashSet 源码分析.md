---
title: Java集合框架（十三）：HashSet 源码分析
date: 2020-04-04 19:12:33
tags: 框架
categories: java集合框架
---
## 1、简述
HashSet**继承于**AbstractSet，实现接口Set，内部使用HashMap来存储数据，数据存储在HashMap的key中，value只是同一个默认值，所以HashSet存储的值是**不能重复**的。
## 2、HashSet实现2、HashSet实现
HashSet几乎实现了Set接口中的**所有**方法，在此我们不对每一个方法进行介绍，只介绍一部分重要方法和属性。首先我们看一下HashSet的构造方法，了解一下HashSet是如何构建的
![](https://s1.ax1x.com/2020/04/04/GwUci6.png)
```java
 private transient HashMap<E,Object> map;

    // PRESENT则是用来虚拟一个假的value，作为Map的值
    private static final Object PRESENT = new Object();

    /**
     * 使用HashMap构造一个空的HashSet对象，初始容量为16，负载因子为0.75，相当于初始化一个空的HashMap对象
     */
    public HashSet() {
        map = new HashMap<>();
    }

    /**
     * 构造包含指定集合中的元素的新集合
     * 创建一个HashMap对象，默认负载因子是0.75，容量大小取指定集合和默认初始大小16中最大值
     */
    public HashSet(Collection<? extends E> c) {
        map = new HashMap<>(Math.max((int) (c.size()/.75f) + 1, 16));
        addAll(c);
    }

    /**
     * 根据指定容量大小和指定的负载因子，基于HashMap创建一个空的HashSet
     */
    public HashSet(int initialCapacity, float loadFactor) {
        map = new HashMap<>(initialCapacity, loadFactor);
    }

    /**
     * 根据指定初始容量大小，基于HashMap创建一个空的HashSet，默认负载因子为0.75
     */
    public HashSet(int initialCapacity) {
        map = new HashMap<>(initialCapacity);
    }

```
## 3、常用方法

 ```java
   /**
     * 返回集合中元素数量，注意map.size()，实际是map中元素的数量
     */
    public int size() {
        return map.size();
    }
    /**
     * 判断集合中是否抱恨指定的元素，实际判断map中是否包含指定元素作为key的对象
     */
    public boolean contains(Object o) {
        return map.containsKey(o);
    }

    /**
     *将指定元素添加到集合中，该元素作为map中的key，PRESENT作为值
     */
    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }

    /**
     * 从集合中移除指定的元素
     */
    public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }
```
## 4、HashSet元素不重复原理
由源码分析中add()方法可知，HashSet集合中添加元素，实际是作为HashMap的Key存储，
由于 HashMap 的 put() 方法添加 key-value 时，当新放入 HashMap 的 Entry 中 key 与集合中原有 Entry 的 key 相同（hashCode()返回值相等，通过 equals 比较也返回 true），新添加的 Entry 的 value 会将覆盖原来 Entry 的 value，但 key 不会有任何改变，因此如果向 HashSet 中添加一个已经存在的元素时，新添加的集合元素将不会被放入 HashMap中，原来的元素也不会有任何改变，这也就满足了 Set 中元素不重复的特性。

如果添加元素在 HashSet 中不存在的，则返回 true；如果添加的元素已经存在，返回 false。其原因在于 HashMap 的 put 方法。该方法在添加 key 不重复的键值对的时候，会返回 null。


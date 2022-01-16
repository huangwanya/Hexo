---
title: Java集合框架（十四）：LinkedHashSet 源码分析
date: 2020-04-04 19:26:46
tags: 框架
categories: java集合框架
---
## 1、LinkedHashSet 简述
LinkedHashSet底层使用 LinkedHashMap 来**保存所有元素**，它继承自 HashSet，其所有的方法操作上又与 HashSet 相同，因此 LinkedHashSet 的实现上非常简单，只提供了**四个构造方法**，并通过传递一个标识参数，调用父类的构造器，底层构造一个 LinkedHashMap 来实现，在相关操作上与父类 HashSet 的操作相同，直接调用父类 HashSet 的方法
![](https://s1.ax1x.com/2020/04/04/GwUHFP.png)
## 2、LinkedHashSet实现
因为LinkedHashSet都是调用父类的方法，在此我们只介绍他的构造函数

```java
public class LinkedHashSet<E>
    extends HashSet<E>
    implements Set<E>, Cloneable, java.io.Serializable {

    private static final long serialVersionUID = -2851667679971038690L;

    /**
     * 构造一个带有指定初始容量和加载因子的新空链表哈希set。
     *
     * 底层会调用父类的构造方法，构造一个有指定初始容量和负载因子的LinkedHashMap实例。
   ```java
  * @param initialCapacity 初始容量
     * @param loadFactor 负载因子
     */
    public LinkedHashSet(int initialCapacity, float loadFactor) {
        super(initialCapacity, loadFactor, true);
    }

    /**
     * 构造一个带指定初始容量和默认负载因子0.75的新空链表哈希set。
     *
     * 底层会调用父类的构造方法，构造一个带指定初始容量和默认负载因子0.75的LinkedHashMap实例。
     * @param initialCapacity 初始容量。
     */
    public LinkedHashSet(int initialCapacity) {
        super(initialCapacity, .75f, true);
    }

    /**
     * 构造一个带默认初始容量16和负载因子0.75的新空链接哈希set。
     *
     * 底层会调用父类的构造方法，构造一个带默认初始容量16和负载因子0.75的LinkedHashMap实例。
     */
    public LinkedHashSet() {
        super(16, .75f, true);
    }

    /**
     * 构造一个与指定collection中的元素相同的新链表哈希set。
     *
     * 底层会调用父类的构造方法，构造一个足以包含指定collection
     * 中所有元素的初始容量和负载因子为0.75的LinkedHashMap实例。
     * @param c 指定集合
     */
    public LinkedHashSet(Collection<? extends E> c) {
        super(Math.max(2*c.size(), 11), .75f, true);
        addAll(c);
    }
}
```

我们看到构造函数中都是调用父类的构造函数，接着我们看一下其调用的父类的构造函数

```java
/**
     * 根据指定的initialCapacity和loadFactor构造一个新的空链表哈希集合。
     * 此构造函数访问权限为包访问权限，实际只是对LinkedHashSet的开放
     *
     * 底层实现会以指定的参数构造一个空LinkedHashMap实例来实现
     * @param initialCapacity 初始容量。
     * @param loadFactor 负载因子
     * @param dummy 标识
     */
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
    map = new LinkedHashMap<E,Object>(initialCapacity, loadFactor);
}

```
## 3、LinkedHashSet总结
由源码我们可以看出LinkedHashSet其实是对LinkedHashMap的包装，其底层实现完全依靠LinkedHashMap，因此想要完全理解LinkedHashSet，只需要清楚**LinkedHashMap**的实现原理即可

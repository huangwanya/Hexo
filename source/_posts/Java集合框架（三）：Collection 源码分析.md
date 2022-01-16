---
title: Java集合框架（三）：Collection 源码分析
date: 2020-03-30 19:22:45
tags: 框架
categories: java集合框架
---
## 前言：
本文我们主要学习Java集合框架的根接口**Collection**，通过本文我们可以进一步了解Collection的属性及提供的方法。在介绍Collection接口之前我们不得不先学习一下**Iterable**，因为Collection接口**继承**了它。

## Collection接口类图
从类图中我们看到Collection接口继承了Iterable接口
![](https://s1.ax1x.com/2020/03/30/GnI0qx.png)

## Iterable源码分析
由类图我们可以发现，Iterable接口有**三个方法**，其中两个方法使用default修饰的，这是jdk1.8的新特性，允许接口中有默认实现方法，其实现类中可以有选择的重写这部分方法，下面我们看一下每个方法的作用是什么：
```java
#返回元素类型为T的迭代器，有了这个迭代器就可以对集合中元素进行遍历
Iterator<T> iterator();

#遍历集合中的元素并对元素进行执行操作，直到遍历完成或抛出异常
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
         action.accept(t);
    }
}

#Spliterator（splitable iterator可分割迭代器），对集合进行并行遍历
default Spliterator<T> spliterator() {
        return Spliterators.spliteratorUnknownSize(iterator(), 0);
}

```
## Iterable接口总结：
默认为所有集合类添加了两个方法，
一个是**遍历并处理集合元素**
一个是**可分割迭代器，与方法iterator相比，一个是顺序遍历，一个是并行遍历**

## Collection接口源码分析
```java
    //查询操作，以下方法属于查询操作
    #返回集合中元素数量，如果元素数量大于Integer.MAX_VALUE将返回Integer.MAX_VALUE
    int size();

    #如果集合中没有元素，将返回true，否则返回false
    boolean isEmpty();

    #如果集合中包含指定的元素，则返回true，否则false
    boolean contains(Object o);

    #返回该集合中元素的迭代器。对于返回元素的顺序，没有任何保证。
    Iterator<E> iterator();

    #如果此集合对其迭代器返回的元素的顺序有任何保证，则该方法必须以相同的顺序返回元素。
    #这个方法必须分配一个新数组，即使这个集合是由数组组成的。
    #这个方法充当集合与数组之间转换的桥梁
    Object[] toArray();

    #返回一个包含集合中所有元素的数组
    #如果集合中的元素类型与指定数组类型相匹配则直接按照元素中数组类型返回
    #否则，将使用指定数组的运行时类型和该集合的大小分配新的数组。
    #如果此集合对其迭代器返回的元素的顺序有任何保证，则该方法必须以相同的顺序返回元素。
    #T:返回数组的类型
    #a:如果该数组足够大可以容纳集合中所有元素，则使用该数组存储，否则将根据集合大小和元素类型重新创建一个数组
    <T> T[] toArray(T[] a);

    // 修改操作，以下方法属于修改操作

    #向集合中添加元素，如果该元素在集合中已经存在则返回false，否则返回true
    #集合本身对添加的元素会有所限制，比如有些集合拒绝添加null
    boolean add(E e);

    #从集合中删除指定元素，如果指定的元素存在集合中并完成了删除操作，则返回true
    boolean remove(Object o);


    // 批量操作

     #如果集合中包含指定的所有元素，则返回true
    boolean containsAll(Collection<?> c);

    #将指定集合中的所有元素添加到此集合
    #如果在运行过程中修改指定的集合，则该操作的行为是未定义的
    boolean addAll(Collection<? extends E> c);

    #从当前集合中移除指定集合中的所有元素
    boolean removeAll(Collection<?> c);

   
    # @since 1.8
    #删除该集合中满足给定谓词的所有元素
    #在迭代期间抛出的任何异常都会传递给调用者
    default boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        boolean removed = false;
        final Iterator<E> each = iterator();
        while (each.hasNext()) {
            if (filter.test(each.next())) {
                each.remove();
                removed = true;
            }
        }
        return removed;
    }

    #只保留集合中指定的元素
    boolean retainAll(Collection<?> c);

    #清空集合中所有元素
    void clear();


    // 比较和散列

    #比较指定对象与当前集合是否相等
    #@see Object#equals(Object)
    #@see Set#equals(Object)
    #@see List#equals(Object)
    boolean equals(Object o);

    #@see Object#hashCode()
    #@see Object#equals(Object)
    #返回当前对象的哈希值
    int hashCode();

    
    #@since 1.8
    #创建一个集合的并行迭代器
    @Override
    default Spliterator<E> spliterator() {
        return Spliterators.spliterator(this, 0);
    }

   
    #@since 1.8
    #将当前集合作为数据源，创建一个序列流
    default Stream<E> stream() {
        return StreamSupport.stream(spliterator(), false);
    }

    #@since 1.8
    #将当前集合作为数据源，创建一个可并行计算的序列流
    default Stream<E> parallelStream() {
        return StreamSupport.stream(spliterator(), true);
    }

```
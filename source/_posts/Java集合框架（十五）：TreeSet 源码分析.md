---
title: Java集合框架（十五）：TreeSet 源码分析
date: 2020-04-04 19:38:33
tags: 框架
categories: java集合框架
---
## 1、TreeSet 简述
🌂TreeSet是基于TreeMap作为存储的可排序、可去重的有序集合

🌂继承于AbstractSet,AbstractSet实现了equals和hashcode方法

🌂实现了NavigableSet接口，意味着它支持一系列的导航方法，比如查找与指定目标最匹配项

🌂实现了Cloneable接口，意味着它能被复制

🌂实现了java.io.Serializable接口，意味着它支持序列化

![](https://s1.ax1x.com/2020/04/04/GwUIeA.png)

2、TreeSet源码分析
```java
public class TreeSet<E> extends AbstractSet<E>
    implements NavigableSet<E>, Cloneable, java.io.Serializable
{
    /**
     * 使用 NavigableMap 的 key 来存储 Set 集合的元素
     */
    private transient NavigableMap<E,Object> m;

    // 虚拟一个 PRESENT 作为 Map 集合的所有 value
    private static final Object PRESENT = new Object();

    /**
     * 包级别访问权限的构造器，以指定的 NavigableMap 对象创建 Set 集合
     */
    TreeSet(NavigableMap<E,Object> m) {
        this.m = m;
    }

    /**
     * 以自然排序方式创建一个新的 TreeMap，
     * 使用该 TreeMap 的 key 来保存 Set 集合的元素
     */
    public TreeSet() {
        this(new TreeMap<E,Object>());
    }

    /**
     * 根据自定义排序方式创建一个新的 TreeMap，
     * 使用该 TreeMap 的 key 来保存 Set 集合的元素
     */
    public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
    }

    /**
     * 根据无参构造器创建一个 TreeSet，底层以 TreeMap 保存集合元素
     * 向 TreeSet 中添加 指定Collection 集合 c 里的所有元素
     */
    public TreeSet(Collection<? extends E> c) {
        this();
        addAll(c);
    }

    /**
     * 根据有参构造器创建一个 TreeSet，底层以 TreeMap 保存集合元素
     * 向 TreeSet 中添加 指定Collection 集合 c 里的所有元素
     */
    public TreeSet(SortedSet<E> s) {
        this(s.comparator());
        addAll(s);
    }

  
---------------下面来看addAll方法-------------------
    public  boolean addAll(Collection<? extends E> c) {
        // Use linear-time version if applicable
        if (m.size()==0 && c.size() > 0 &&
            c instanceof SortedSet &&
            m instanceof TreeMap) {
            # 把指定集合强制转换为 SortedSet 集合
            SortedSet<? extends E> set = (SortedSet<? extends E>) c;
            # 把NavigableMap集合强制转换为 TreeMap集合
            TreeMap<E,Object> map = (TreeMap<E, Object>) m;
            Comparator<?> cc = set.comparator();
            Comparator<? super E> mc = map.comparator();
            # 如果 cc 和 mc 两个 Comparator 相等
            if (cc==mc || (cc != null && cc.equals(mc))) {
            # 把指定集合Collection 中所有元素作为 TreeMap 集合的 key 进行存储
                map.addAllForTreeSet(set, PRESENT);
                return true;
            }
        }
        return super.addAll(c);
    }
}
```
从上面代码可以看出，TreeSet 的前两个构造器的都是新建一个 TreeMap 作为实际存储 Set 元素的**容器**，而另外 2 个构造器则分别**依赖**于前两个构造器，由此可见，TreeSet 底层实际使用的存储容器就是 TreeMap

## 3、TreeSet排序方式
**由源码我们可知TreeSet的排序方式有两种：**
### 3.1、一是自然排序，使用默认构造函数3.1、一是自然排序，使用默认构造函数

Java提供了一个Comparable接口，该接口里定义了一个compareTo(Object obj)方法，该方法返回一个整数值，实现该接口的类必须实现该方法，实现了该接口的类的对象就可以比较大小了。当一个对象调用该方法与另一个对象进行比较，

例如 obj1.compareTo(obj2)

🌂如果该方法返回0，则表明这两个对象相等；

🌂如果该方法返回一个正整数，则表明obj1大于obj2；

🌂如果该方法返回一个负整数，则表明obj1小于obj2。

Java的一些常用类已经实现了Comparable接口，并提供了比较大小的标准， 下面是实现了Comparable接口的**常用类**：

🌂BigDecimal、BigInteger以及所有数值型对应包装类：按它们对象的数值大小进行比较

🌂Character ：按字符的Unicode值进行比较

🌂Boolean ： true对应的包装类实例大于false对应的包装类实例

🌂String : 按字符串中字符的Unicode值进行比较

🌂Date、Time ： 后面的时间、日期比前面的日期时间大。

### 3.2、二是比较器排序，使用指定比较器的构造函数3.2、二是比较器排序，使用指定比较器的构造函数

TreeSet的自然排序是根据集合元素的大小默认以**升序排列**。如果需要完成定制排序，例如以降序排列，则可以使用Comparator接口的帮助。
该接口里包含了一个int compare(T o1, T o2)方法，

**该方法用于比较o1、o2的大小：**
🌂如果该方法返回正整数，则表明o1大于o2；

🌂如果该方法返回0，则表明o1等于o2；

🌂如果该方法返回负整数，则表明o1小于o2。

## 4、示例
### 4.1、以自然顺序排序，并且元素不重复4.1、以自然顺序排序，并且元素不重复
```java
public class TreeSetDemo {
    public static void main(String[] args) {
        TreeSet<String> treeSet = new TreeSet<>();
        for (int i = 0; i < 5; i++) {
            treeSet.add("item" + i);
        }
        for (int i = 0; i < 5; i++) {
            treeSet.add("item" + i);
        }

        treeSet.forEach(item -> {
            System.err.println(item);
        });

    }
}
```
**输出结果：**
```java
item0
item1
item2
item3
item4
```
### 4.2、按比较器排序
指定一个比较器，倒置集合中的元素
```java
public class TreeSetDemo {
    public static void main(String[] args) {
        //指定一个比较器，倒置元素顺序
        TreeSet<String> treeSet = new TreeSet<>(Comparator.reverseOrder());
        for (int i = 0; i < 5; i++) {
            treeSet.add("item" + i);
        }
        for (int i = 0; i < 5; i++) {
            treeSet.add("item" + i);
        }

        treeSet.forEach(item -> {
            System.err.println(item);
        });

    }
}
```
**输出结果：**
```java
item4
item3
item2
item1
item0
```
## 5、HashSet和TreeSet区别5、HashSet和TreeSet区别
1）HashSet基于HashMap实现，HashSet里面的元素是无序的且不重复的
2）HashSet允许使用null，有且仅有一个元素为null
3）TreeSet基于TreeMap实现，是一个有序的集合
4）TreeSet中不允许使用null元素
5）TreeSet中的元素支持2种排序方式：自然排序 或者 根据 Comparator 进行排序
6）TreeSet不支持快速随机遍历，只能通过迭代器进行遍历！ 
7）HashSet和TreeSet都是非同步的，在使用Iterator进行迭代的时候要注意fail-fast
## 6、总结6、总结
🌂TreeSet添加元素的时候，如果元素本身具备了自然顺序的特性，那么就按照元素自然顺序的特性进行排序存储。

🌂TreeSet添加元素的时候，如果元素本身不具备自然顺序的特性，那么该元素所属的类必须要实现Comparable接口，把元素的比较规则定义在compareTo(T o)方法上。如果比较元素的时候，compareTo方法返回 的是0，那么该元素就被视为重复元素，不允许添加

🌂TreeSet添加元素的时候, 如果元素本身没有具备自然顺序 的特性，并且元素所属的类也没有实现Comparable接口，那么必须要在创建TreeSet的时候传入一个比较器。

🌂TreeSet添加元素的时候，如果元素本身不具备自然顺序的特性，而元素所属的类已经实现了Comparable接口，在创建TreeSet对象的时候又传入了比较器，此时以比较器的比较规则优先使用。

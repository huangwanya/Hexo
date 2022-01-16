---
title: Java集合框架（四）：Iterator 源码分析
date: 2020-03-30 20:08:05
tags: 框架
categories: java集合框架
---
## 1、Java Iterator 简述

Iterator迭代器的定义：**迭代器**（Iterator）模式，又叫做**游标**（Cursor）模式。GOF给出的定义是，提供一种方法访问一个容器（container）对象中各个元素，而又不需暴露该对象的内部细节。迭代器通常被称为“**轻量级**”对象，因为创建它的代价小。

在Java中，Iterator是 java.util 包中** Collection** 框架中的接口。 它是一个Java Cursor，用于迭代一组对象。
🌂它用于逐个遍历集合对象元素。
🌂它自 Java 1.2 被引入使用
🌂它适用于所有 Collection 类。 所以它也被称为通用 Java Cursor。
🌂它支持READ和REMOVE操作

## 2、Java 4种 Cursors
Java Cursor 是一个**迭代器**，用于逐个迭代或遍历或检索Collection或Stream对象的元素。
Java支持以下**四种**不同的** Cursor**
![](https://s1.ax1x.com/2020/03/30/GnIws1.png)
## 3、Java Iterator 类图
如下面的类图所示，Java Iterator有四种方法。 我们已经熟悉前四种方法。 Oracle公司在Java SE 8版本中为此接口添加了第四种方法。
![](https://s1.ax1x.com/2020/03/30/GnIUz9.png)
## 4、Java Iterator 方法
```java
public interface Iterator<E> {
    /**
     * 检查集合中是否还有元素
     */
    boolean hasNext();

    /**
     * 返回迭代的下一个元素
     * @throws NoSuchElementException 如果没有可迭代的元素将抛出异常
     */
    E next();

    /**
     *将迭代器新返回的元素删除
     */
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }

    /**
     * 对每个剩余元素执行给定操作，直到所有元素都被处理或操作抛出异常。
     * 如果指定了该顺序，则操作按迭代顺序执行
     * @since 1.8
     */
    default void forEachRemaining(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        while (hasNext())
            action.accept(next());
    }

```
## 5、Iterator使用示例

简单的应用事例，对一个元素为字符串类型的List集合进行遍历，我们显式获取集合的迭代器进行遍历，只将第一个元素直接输出，其余元素做指定的处理
```java
public class IteratorDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            list.add("item" + i);
        }
        //获取迭代器
        Iterator<String> listIterator = list.iterator();
        //判断是否还有元素
        while (listIterator.hasNext()) {
            System.err.println(listIterator.next());
            //对剩下的元素执行指定操作
            listIterator.forEachRemaining((String consumer) -> {
                System.err.println(consumer.concat("-test"));
            });
        }

    }
}


```
结果：
```java
item0
item1-test
item2-test
item3-test
item4-test

```
## 6、自定义类迭代器

定义一个对象
```java
public class Employee {

    private int empid;
    private String ename;
    private String designation;
    private double salary;

    public Employee(int empid, String ename, String designation, double salary) {
        this.empid = empid;
        this.ename = ename;
        this.designation = designation;
        this.salary = salary;
    }

    public int getEmpid() {
        return empid;
    }

    public String getEname() {
        return ename;
    }

    public String getDesignation() {
        return designation;
    }

    public double getSalary() {
        return salary;
    }

    @Override
    public String toString() {
        return empid + "\t" + ename + "\t" + designation + "\t" + salary;
    }

}

```
定义一个对象集合 Employees 并实现 Iterable
```java
public class Employees implements Iterable {

    private List<Employee> emps = null;

    public Employees() {
        emps = new ArrayList<>();
        emps.add(new Employee(1001, "黄一", "组长", 250000L));
        emps.add(new Employee(1002, "李二", "开发", 150000L));
        emps.add(new Employee(1003, "王三", "测试", 150000L));
    }

    @Override
    public Iterator<Employee> iterator() {
        return emps.iterator();
    }

}


```
测试对象迭代 EmployeesTest
```java
public class EmployeesTest {
    public static void main(String[] args) {
        Employees emps = new Employees();
        for(Object emp : emps){
            System.out.println(emp);
        }
    }
}

```
**输出**
```java
1001	黄一	组长	250000.0
1002	李二	开发	150000.0
1003	王三	测试	150000.0

```
## 7、Java Iterator 内部是如何工作的？

我们将尝试分析 **Java Iterator** 及其方法如何在内部工作的，在此 我们借助** LinkedList** 对象来理解此功能。

首先，初始化一个对象** LinkedList**
```java
List<String> names = new LinkedList<>();
names.add("E-1");
names.add("E-2");
names.add("E-3");
................
names.add("E-n");

```
获取对象** names** 的迭代器
```java
Iterator<String> namesIterator = names.iterator();

```


我们假设，此时 **namesIterator** 迭代器如下，此时迭代器的 cursor 指向第一元素之前的位置：
![](https://s1.ax1x.com/2020/03/30/GnINRJ.png)
我们执行如下代码：

  ```java
namesIterator.hasNext();
  namesIterator.next();
```

此时 namesIterator 的 cursor 已经指向了第一个元素，如下图：
![](https://s1.ax1x.com/2020/03/30/GnIYiF.png)
我们再次执行如下代码，此时 namesIterator 的 cursor 会执行下一个元素，我们循环此操作，当 cursor 指向最后一个元素时，namesIterator.hasNext() 将会返回 false，此时元素迭代完成，如下图：

 ```java
 namesIterator.hasNext();
  namesIterator.next();
```
![](https://s1.ax1x.com/2020/03/30/GnIdMR.png)
在观察了以上过程之后，我们会发现 Java Iterator 仅支持 Forward Direction Iteration（正向迭代），如下图所示。 所以它也被称为正向迭代。
![](https://s1.ax1x.com/2020/03/30/Gnjlp6.png)
## 8、Iterator总结

Java中的Iterator功能比较简单，并且只能单向移动：

🌂 方法iterator()将返回一个Iterator。首次调用next()方法时，它将返回第一个元素

🌂 next() 返回下一个元素

🌂 hasNext() 检查集合中是否还有元素

🌂 remove() 将迭代器新返回的元素删除

🌂 forEachRemaining() 对每个剩余的元素执行指定的操作
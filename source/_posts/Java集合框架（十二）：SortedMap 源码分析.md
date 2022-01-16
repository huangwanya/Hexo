---
title: Java集合框架（十二）：SortedMap 源码分析
date: 2020-04-04 19:00:41
tags: 框架
categories: java集合框架
---
## 1、SortedMap 简述
SortedMap 接口**扩展了 Map 接口**并提供了有序的Map实现，SortedMap 的排序方式有**两种**：根据键值的自然顺序排序和指定比较器（Comparator）排序。插入有序的 SortedMap 的所有元素都必须实现Comparable接口

所有通用的有序映射实现类都应该提供四个“标准”构造函数：
🌂一个void（无参数）构造函数，它根据键的自然顺序创建一个空的有序映射。
🌂具有Comparator类型的单个参数的构造函数，它创建根据指定的比较器排序的空的有序映射。
🌂具有Map类型的单个参数的构造函数，它创建一个具有与其参数相同的 key-value 映射的新映射，并根据键的自然顺序进行排序。
🌂具有SortedMap类型的单个参数的构造函数，它创建一个新的有序映射，其具有相同的 key-value 映射和与输入有序映射相同的顺序。
![](https://s1.ax1x.com/2020/04/04/GwUTot.png)
## 2、SortedMap 类图
![](https://s1.ax1x.com/2020/04/04/GwUBL9.png)

🌂父接口:
Map<K,V>

🌂所有已知子接口:
ConcurrentNavigableMap<K,V>, NavigableMap<K,V>

🌂所有已知实现类:
ConcurrentSkipListMap, TreeMap
## 3、SortedMap 方法说明

|Queue 方法|等价 Deque 方法|
| ------ |  ------ |
|Comparator<? super K> comparator()|回用于对此映射中的键进行排序的比较器;如果此映射使用其键的自然顺序，则返回null。
|SortedMap<K,V> subMap(K fromKey, K toKey)|回此映射部分的视图，其键的范围从fromKey（包括）到 toKey（不包括）。 （如果fromKey和 toKey 相等，则返回的视图为空。）当尝试在返回的映射范围之外插入键时，将会抛出 IllegalArgumentException。
|SortedMap<K,V> headMap(K toKey)|回此映射的部分视图，其键的范围小于 toKey。
|SortedMap<K,V> tailMap(K fromKey)|回此映射的部分视图，其键的范围大于等于fromKey。
|K firstKey()|回此映射中当前的第一个键。
|K lastKey()|回此映射中当前的最后一个键。
|Set keySet()|回此映射中所有键的 Set 视图。 set的迭代器按升序返回键。如果在对集合进行迭代时修改了映射（除了通过迭代器自己的remove操作），迭代的结果是未知的。 该集合支持元素删除，它支持通过 Iterator.remove，Set.remove，removeAll，retainAll 和 clear 操作从视图中删除相应的映射。 它不支持add或addAll操作。
|Collection values()|回此映射中包含的值的Collection视图。 集合的迭代器以相应键的升序返回值。如果在对集合进行迭代时修改了映射（除了通过迭代器自己的remove操作），迭代的结果是未知的。 该集合支持元素删除，它支持通过 Iterator.remove，Set.remove，removeAll，retainAll 和 clear 操作从视图中删除相应的映射。 它不支持add或addAll操作。
|Set<Map.Entry<K,V>> entrySet()|返回此映射中包含的映射的Set<Map.Entry<K,V>>视图。 set的迭代器以升序键顺序返回条目。如果在对集合进行迭代时修改了映射（除了通过迭代器自己的remove操作），迭代的结果是未知的。 该集合支持元素删除，它支持通过 Iterator.remove，Set.remove，removeAll，retainAll 和 clear 操作从视图中删除相应的映射。 它不支持add或addAll操作。

## 4、SortedMap 应用示例
以下示例需要使用的对象如下
```java
public class PersonDetail {
    String name;
    LocalDate birthday;
    String address;

    public PersonDetail(String name, LocalDate birthday, String address) {
        this.name = name;
        this.birthday = birthday;
        this.address = address;
    }

    @Override
    public String toString() {
        return this.name + ", from " + this.address;
    }
}
```
### 4.1、SortedMap 构造函数示例
#### 4.1.1、无参构造函数
按照key的自然顺序排序

```java
private static void method1() {
        //key的自然顺序排序
        SortedMap<String, PersonDetail> personMap = new TreeMap<>();
        personMap.put("zhangsan", new PersonDetail("张三", LocalDate.of(1998, 6, 22), "小胡同3号"));
        personMap.put("lisi", new PersonDetail("李四", LocalDate.of(1996, 2, 2), "小胡同8号"));
        personMap.put("wanger", new PersonDetail("王二", LocalDate.of(2010, 12, 11), "小胡同28号"));

        personMap.forEach((key, value) -> System.out.println(key + " -> " + value));
    }
```
**输出结果：**
```java
lisi -> 李四, from 小胡同8号
wanger -> 王二, from 小胡同28号
zhangsan -> 张三, from 小胡同3号
```
#### 4.1.2、具有 Comparator 的构造函数

我们编写一个lambda表达式来提供Comparator接口的compareTo实现。 假设我们希望按照key长度按降序对key进行排序

```java
private static void method1() {
        //key的长度排序
        SortedMap<String, PersonDetail> personMap = new TreeMap<>((s1, s2) -> s2.length() - s1.length());
        personMap.put("zhangsan", new PersonDetail("张三", LocalDate.of(1998, 6, 22), "小胡同3号"));
        personMap.put("lisi", new PersonDetail("李四", LocalDate.of(1996, 2, 2), "小胡同8号"));
        personMap.put("wanger", new PersonDetail("王二", LocalDate.of(2010, 12, 11), "小胡同28号"));

        personMap.forEach((key, value) -> System.out.println(key + " -> " + value));
    }
```
**输出结果：**
```java
zhangsan -> 张三, from 小胡同3号
wanger -> 王二, from 小胡同28号
lisi -> 李四, from 小胡同8号
```
#### 4.1.3、参数为 Map 类型的构造函数

```java
 private static void method2() {
        //普通map
        Map<String, PersonDetail> generalMap = new HashMap<>();
        generalMap.put("lilei", new PersonDetail("李雷", LocalDate.of(1994, 6, 22), "1号大街"));
        generalMap.put("hanmeimei", new PersonDetail("韩梅梅", LocalDate.of(1995, 2, 2), "2号大街"));
        generalMap.put("zhangmeili", new PersonDetail("张美丽", LocalDate.of(2010, 12, 11), "3号大街"));
        //普通map作为SortedMap构造函数的参数
        SortedMap<String, PersonDetail> sortedMap = new TreeMap<>(generalMap);
        System.err.println("普通map作为SortedMap构造函数的参数：");
        sortedMap.forEach((key, value) -> System.out.println(key + " -> " + value));
}
```
**输出结果：**

```java
普通map作为SortedMap构造函数的参数：
hanmeimei -> 韩梅梅, from 2号大街
lilei -> 李雷, from 1号大街
zhangmeili -> 张美丽, from 3号大街
```
#### 4.1.4、参数为 SortedMap 类型的构造函数
```java
private static void method2() {

        //指定比较器的map
        Map<String, PersonDetail> sortedComparetorMap = new TreeMap<>((s1, s2) -> s2.length() - s1.length());
        sortedComparetorMap.put("lilei", new PersonDetail("李雷", LocalDate.of(1994, 6, 22), "1号大街"));
        sortedComparetorMap.put("hanmeimei", new PersonDetail("韩梅梅", LocalDate.of(1995, 2, 2), "2号大街"));
        sortedComparetorMap.put("zhangmeili", new PersonDetail("张美丽", LocalDate.of(2010, 12, 11), "3号大街"));
        
        System.err.println("指定比较器的有序map作为SortedMap构造函数的参数：");
        SortedMap<String, PersonDetail> newMap = new TreeMap<>(sortedComparetorMap);
        newMap.forEach((key, value) -> System.out.println(key + " -> " + value));

    }
```
**输出结果：**
```java
指定比较器的有序map作为SortedMap构造函数的参数：
hanmeimei -> 韩梅梅, from 2号大街
lilei -> 李雷, from 1号大街
zhangmeili -> 张美丽, from 3号大街
```
### 4.2、SortedMap 方法示例
```java
private static void method3() {
        SortedMap<String, PersonDetail> personMap = new TreeMap<>((s1, s2) -> s2.length() - s1.length());
        personMap.put("zhangsan", new PersonDetail("张三", LocalDate.of(1998, 6, 22), "小胡同3号"));
        personMap.put("lisi", new PersonDetail("李四", LocalDate.of(1996, 2, 2), "小胡同8号"));
        personMap.put("wanger", new PersonDetail("王二", LocalDate.of(2010, 12, 11), "小胡同28号"));

        System.out.println("映射关系如下：===================");
        personMap.forEach((key, value) -> System.out.println(key + " -> " + value));
        System.out.println("===================");

        System.out.println("entrySet:" + personMap.entrySet());
        System.out.println("firstKey:" + personMap.firstKey());
        System.out.println("lastKey:" + personMap.lastKey());
        System.out.println("keySet:" + personMap.keySet());
        System.out.println("values:" + personMap.values());
        System.out.println("headMap:" + personMap.headMap("wanger"));
        System.out.println("tailMap:" + personMap.tailMap("wanger"));

    }
```
**输出结果：**
```java
映射关系如下：===================
zhangsan -> 张三, from 小胡同3号
wanger -> 王二, from 小胡同28号
lisi -> 李四, from 小胡同8号
===================
entrySet:[zhangsan=张三, from 小胡同3号, wanger=王二, from 小胡同28号, lisi=李四, from 小胡同8号]
firstKey:zhangsan
lastKey:lisi
keySet:[zhangsan, wanger, lisi]
values:[张三, from 小胡同3号, 王二, from 小胡同28号, 李四, from 小胡同8号]
headMap("wanger"):{zhangsan=张三, from 小胡同3号}
tailMap("wanger"):{wanger=王二, from 小胡同28号, lisi=李四, from 小胡同8号}

```
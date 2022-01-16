---
title: Java集合框架（十一）：Map 源码分析
date: 2020-04-04 18:43:46
tags: 框架
categories: java集合框架
---
## 1、Map 简述

java.util.Map 接口表示**键和值**之间的**映射对象**。Map接口不是Collection接口的子类型。因此它的行为与其他集合类型略有不同。Map接口取代了传统的 Dictionary类，它是一个完全抽象的类而不是接口。

Map 接口具有以下几个特征：

🌂Map提供了三个集合视图，键集，键值映射集和值集合。

🌂Map 不能包含重复的键，每个键最多可以映射一个值。一些实现允许null键和null值，如HashMap和LinkedHashMap，但有些实现不允许，比如：TreeMap。

🌂Map 的顺序取决于具体的实现，例如 TreeMap 和LinkedHashMap 保证了元素的顺序，而 HashMap 则没有。

🌂Map 使用 hashCode 和equals 方法来获取和存放操作。 所以可变类不适合Map键。 如果 hashCode 或 equals 的值在put之后发生变化，则在get操作中将无法获得正确的值。

🌂AbstractMap 类提供了 Map 接口的主要实现，大多数Map实现类扩展了 AbstractMap 类并实现了所需的方法。

🌂当访问的值不存在的时候，方法就会抛出一个NoSuchElementException异常.

🌂当对象的类型和Map里元素类型不兼容的时候，就会抛出一个 ClassCastException异常。

🌂当在不允许使用Null对象的Map中使用Null对象，会抛出一个NullPointerException 异常。

🌂当尝试修改一个只读的Map时，会抛出一个UnsupportedOperationException异常。

🌂在Java中有两个常用的实现接口：Map 和 SortedMap，以及三个常用的实现类：HashMap，TreeMap 和 LinkedHashMap。

![](https://s1.ax1x.com/2020/04/04/GwU4Ld.png)
## 2、Map 类图
![](https://s1.ax1x.com/2020/04/04/GwUbJf.png)


**已知Map子接口：**
🌂Bindings
🌂ConcurrentMap<K,V>
🌂ConcurrentNavigableMap<K,V>
🌂LogicalMessageContext
🌂MessageContext
🌂NavigableMap<K,V>
🌂SOAPMessageContext
🌂SortedMap<K,V>

**已知Map实现类：**
🌂AbstractMap
🌂Attributes
🌂AuthProvider
🌂ConcurrentHashMap
🌂ConcurrentSkipListMap
🌂EnumMap
🌂HashMap
🌂Hashtable
🌂IdentityHashMap
🌂LinkedHashMap
🌂PrinterStateReasons
🌂Properties
🌂Provider
🌂RenderingHints
🌂SimpleBindings
🌂TabularDataSupport
🌂TreeMap
🌂UIDefaults
🌂WeakHashMap
## 3、Map 方法说明

|Queue 方法|等价 Deque 方法|
| ------ |  ------ |
|nt size()|回此映射中键 - 值映射的数量。 如果映射数量大于 Integer.MAX_VALUE，则返回 Integer.MAX_VALUE。
|oolean isEmpty()|此映射不包含键 - 值映射，则返回 true。
|oolean containsKey(Object key)|果此映射包含指定键的映射，则返回 true。
|oolean containsValue(Object value)|果此映射将一个或多个键映射到指定值，则返回 true。
| get(Object key)|回指定键所映射的值；如果此映射不包含该键的映射关系，则返回 null。
| put(K key, V value)|指定的值与此映射中的指定键相关联。 如果映射到先前包含键的映射，则旧值将替换为指定的值。 （当且仅当m.containsKey（k）返回true时。）
| remove(Object key)|果存在键，则从该映射中移除键的映射。换句话说，如果此映射包含键 k 到值 v 的映射，使得（key == null？k == null：key.equals（k）），则删除该映射。
|oid putAll(Map<? extends K,? extends V> m)|指定Map集合中的所有映射复制到当前Map集合，此调用的效果等同于在当前Map上调用put（k，v）的效果。 如果在操作过程中修改了指定的Map，则此操作的行为是不确定的。
|oid clear()|Map映射中删除所有映射。
|et keySet()|回此映射中包含的键的Set视图。 因此对Map的更改将反映在集合中，反之亦然。 如果在对集合进行迭代时，修改了映射（除了通过迭代器自己的remove操作），迭代的结果是未知的。 该集合支持元素删除，它通过 Iterator.remove，Set.remove，removeAll，retainAll和clear操作从Map中删除相应的映射, 它不支持add或addAll操作。
|ollection values()|回此映射中包含的值的Collection视图。 如果在对集合进行迭代时修改了映射（除了通过迭代器自己的remove操作），迭代的结果是未知的。 该集合支持元素删除，它通过Iterator.remove，Collection.remove，removeAll，retainAll和clear操作从Map中删除相应的映射。 它不支持add或addAll操作。
|et<Map.Entry<K,V>> entrySet()|回此映射中包含的映射的Set视图。 如果在对集合进行迭代时修改了映射（除非通过迭代器自己的remove操作，或者通过迭代器返回的映射条目上的setValue操作），迭代的结果是未可知的。 该集支持元素删除，它通过 Iterator.remove，Set.remove，removeAll，retainAll和clear操作从Map中删除相应的映射。 它不支持add或addAll操作。
|oolean equals(Object o)|指定对象与此映射进行相等性比较。 如果给定对象也是一个映射，并且两个映射表示相同的映射，则返回true。 也就是说，如果 m1.entrySet().equals(m2.entrySet())，则两个映射m1和m2表示相同的映射。
|nt hashCode()|回此映射的哈希码值。 映射的哈希码被定义为映射的 entrySet（）视图中每个条目的哈希码的总和。 这确保 m1.equals（m2）也就意味着 m1.hashCode（）== m2.hashCode（）。

## 4、JDK 1.8 新增的默认实现方法如下
**default V getOrDefault(Object key,V defaultValue)**
返回指定键映射到的值，如果此映射不包含键的映射，则返回defaultValue。

**default void forEach(BiConsumer<? super K,? super V> action)**
对此映射中的每个条目执行给定操作，直到处理完所有条目或操作引发异常。 除非实现类另有指定，否则将按迭代的顺序执行操作，操作抛出的异常将返回给调用者。
 ```java
for (Map.Entry<K, V> entry : map.entrySet())
     action.accept(entry.getKey(), entry.getValue());
```

**default void replaceAll(BiFunction<? super K,? super V,? extends V> function)**
将每个条目的值替换为在该条目上调用给定函数的结果，直到所有条目都已处理或函数抛出异常。 函数抛出的异常将转发给调用者。
```java

 for (Map.Entry<K, V> entry : map.entrySet())
     entry.setValue(function.apply(entry.getKey(), entry.getValue()));
```

**default V putIfAbsent(K key,V value)**
如果指定的键尚未与值关联（或映射为null），则将其与给定值关联并返回null，否则返回当前值。
```java
 V v = map.get(key);
 if (v == null)
     v = map.put(key, value);

 return v;
```

**default boolean remove(Object key, Object value)**
仅当指定键映射到指定值时才删除该条目。
```java
 if (map.containsKey(key) && Objects.equals(map.get(key), value)) {
     map.remove(key);
     return true;
 } else
     return false;
```
**default boolean replace(K key, V oldValue,V newValue)**
仅当前映射到指定值时，才替换指定键的条目。

 ```java
if (map.containsKey(key) && Objects.equals(map.get(key), value)) {
     map.put(key, newValue);
     return true;
 } else
     return false;
```
**default V replace(K key,V value)**
仅当指定键映射到某个值时才替换该条目的条目。

```java
 if (map.containsKey(key)) {
     return map.put(key, value);
 } else
     return null;
```

**default V computeIfAbsent(K key, Function<? super K,? extends V> mappingFunction)**
如果指定的键尚未与值关联（或映射为null），则尝试使用给定的映射函数计算其值，并将其输入此映射，除非为null。

如果函数返回null，则不记录映射。 如果函数本身抛出（未经检查的）异常，则重新抛出异常，并且不记录映射。

最常见的用法是构造一个新对象，用作初始映射值或记录结果，如下所示：
```java
 map.computeIfAbsent(key, k -> new Value(f(k)));
```
或者实现一个多值映射，Map <K，Collection >，支持每个键的多个值：
```java
 map.computeIfAbsent(key, k -> new HashSet<V>()).add(v);
```
默认实现等效于此映射的以下步骤，然后返回当前值，如果不存在则返回null：

```java
 if (map.get(key) == null) {
     V newValue = mappingFunction.apply(key);
     if (newValue != null)
         map.put(key, newValue);
 }
```
**default V computeIfPresent(K key,BiFunction<? super K,? super V,? extends V> remappingFunction)**
如果指定键的值存在且为非null，则尝试在给定键及其当前映射值的情况下计算新映射。如果函数返回null，则删除映射。 如果函数本身抛出（未经检查的）异常，则重新抛出异常，并保持当前映射不变。
默认实现等效于对此映射执行以下步骤，然后返回当前值，如果不存在则返回null：

```java
 if (map.get(key) != null) {
     V oldValue = map.get(key);
     V newValue = remappingFunction.apply(key, oldValue);
     if (newValue != null)
         map.put(key, newValue);
     else
         map.remove(key);
 }
```

**default V compute(K key, BiFunction<? super K,? super V,? extends V> remappingFunction)**
尝试计算指定键及其当前映射值的映射（如果没有当前映射，则为null）。
例如，要创建或追加 字符串 msg 到值映射：
```java
 map.compute(key, (k, v) -> (v == null) ? msg : v.concat(msg))
```
如果函数返回null，则删除映射。 如果函数本身抛出（未经检查的）异常，则重新抛出异常，并保持当前映射不变。

默认实现等效于为此映射执行以下步骤，然后返回当前值，如果不存在则返回null：

```java
V oldValue = map.get(key);
 V newValue = remappingFunction.apply(key, oldValue);
 if (oldValue != null ) {
    if (newValue != null)
       map.put(key, newValue);
    else
       map.remove(key);
 } else {
    if (newValue != null)
       map.put(key, newValue);
    else
       return null;
 }
```
**default V merge(K key,V value,BiFunction<? super V,? super V,? extends V> remappingFunction)**

如果指定的键尚未与值关联或与null关联，则将其与给定的非空值关联。 否则，将相关值替换为给定重映射函数的结果，或者如果结果为null则删除。 当组合密钥的多个映射值时，该方法可以是有用的。

**例如**，要创建或附加String msg 到值映射：
```java
 map.merge(key, msg, String::concat)
```
如果函数返回null，则删除映射。 如果函数本身抛出（未经检查的）异常，则重新抛出异常，并保持当前映射不变。

默认实现等效于为此映射执行以下步骤，然后返回当前值，如果不存在则返回null：

```java
default V merge(K key, V value,
            BiFunction<? super V, ? super V, ? extends V> remappingFunction) {
        Objects.requireNonNull(remappingFunction);
        Objects.requireNonNull(value);
        V oldValue = get(key);
        V newValue = (oldValue == null) ? value :
                   remappingFunction.apply(oldValue, value);
        if(newValue == null) {
            remove(key);
        } else {
            put(key, newValue);
        }
        return newValue;
    }
```

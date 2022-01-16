---
title: Java集合框架（十六）：ArrayList 源码分析
date: 2020-04-05 19:43:35
tags: 框架
categories: java集合框架
---
## 1、ArrayList 简述

Java ArrayList是List接口的**可调整大小的数组**实现，这意味着它以默认大小开始，并在将更多数据添加到数组列表时**自动扩容**。 除了实现List接口之外，此类还提供了一些方法来操作内部用于存储列表的数组的大小。

ArrayList**不是线程安全**的，多线程环境下可以考虑用 List list = Collections.synchronizedList(new ArrayList(…)); 函数返回一个线程安全的ArrayList类，也可以使用concurrent并发包下的CopyOnWriteArrayList类。

ArrayList 的iterator和listIterator方法返回的迭代器是快速失败的：如果在创建迭代器之后的任何时候对列表进行结构修改，除了通过迭代器自己的remove或add方法之外，**迭代器将抛**出ConcurrentModificationException。 因此，在并发修改的情况下，迭代器快速失败，而不是在未来的未确定时间冒任意，**非确定性的风险**。

## 2、ArrayList 类图
![](https://s1.ax1x.com/2020/04/05/GDInne.png)

ArrayList **继承**了 AbstractList，实现了List, RandomAccess, Cloneable, java.io.Serializable这些接口。

ArrayList** 实现了** RandmoAccess 接口，即提供了随机访问功能。RandmoAccess是java中用来被List实现，为List提供快速访问功能的。在ArrayList中，我们即可以通过元素的索引快速获取元素对象；这就是快速随机访问。

ArrayList** 实现了** Cloneable接口，即覆盖了方法clone()，能被克隆。

ArrayList **实现了** java.io.Serializable接口，这意味着ArrayList支持序列化，能通过序列化去传输。

ArrayList与Vector**不同**，ArrayList 中的操作不是线程安全的。所以，建议在单线程中才使用ArrayList，而在多线程中可以选择Vector或者CopyOnWriteArrayList。

## 3、ArrayList 构造函数

ArrayList 提供了三个构造函数，分别如下：

### 3.1、ArrayList()

```java
ArrayList 默认构造函数会初始化一个容量为10的空列表，这也就意味着ArrayList的默认大小为10.

    /**
     * 构造一个初始容量为10的空列表。
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
```
### 3.2、ArrayList(int initialCapacity)

该构造函数接受一个int类型的数值来初始化 ArrayList，如果指定的初始容量大小为负数，将会抛出异常 IllegalArgumentException

   ```java
 /**
     * 构造具有指定初始容量的空列表。
     *
     * @param  initialCapacity  列表的初始容量
     * @throws IllegalArgumentException 如果指定的初始容量为负数
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
           //新建一个数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```
### 3.3、ArrayList(Collection<? extends E> c)

   ```java
 /**
     * 按照集合的迭代器返回的顺序构造一个包含指定集合元素的列表。
     *
     * @param c 要将其元素放入此列表的集合
     * @throws NullPointerException 如果指定集合为 null
     */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // 替换为空数组。
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```
## 4、ArrayList 源码解析4、ArrayList 源码解析

### 4.1、私有字段说明
ArrayList包含了两个重要的对象：elementData 和 size。

1）elementData 是"Object[]类型的数组"，它保存了添加到ArrayList中的元素。实际上，elementData是个动态数组，我们能通过构造函数 ArrayList(int initialCapacity)来指定它的初始容量为initialCapacity；

如果通过不含参数的构造函数ArrayList()来创建ArrayList，则elementData的容量默认是10。elementData数组的大小会根据ArrayList容量的增长而动态的增长，具体的增长方式，请参考源码分析中的ensureCapacity()函数。

2） size 则是动态数组的实际大小。

#### 4.1.1、AbstractList>modCount

子类使用这个字段是可选的，若子类希望提供fail-fast(快速失败) iterators或者list iterators 可以在方法里使用该方法。

这个变量用于快速判断该实例是否有变化，若在进行迭代的时候有变更，那么就抛出一个并发修改异常(ConcurrentModificationException)。
fail-fast是Java集合的一种错误检测机制。当多个线程对集合进行结构上的改变的操作时，有可能会产生fail-fast机制。

```java
java.util.AbstractList：
//当前列表结构被修改的次数
protected transient int modCount
```
#### 4.1.2、DEFAULT_CAPACITY

  ```java
  /**
     * 默认初始化容量大小为10
     */
    private static final int DEFAULT_CAPACITY = 10;
```
#### 4.1.3、EMPTY_ELEMENTDATA
```java

    /**
     * 用于空实例的共享空数组实例。
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};
```
#### 4.1.4、DEFAULTCAPACITY_EMPTY_ELEMENTDATA

```java
    /**
     * 用于默认大小的空实例的共享空数组实例。 我们将此与EMPTY_ELEMENTDATA区分开来，以便在添加第一个元素时知道要膨胀多少。
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
```
#### 4.1.5、elementData

   ```java
 /**
     * 存储ArrayList元素的数组缓冲区。
ArrayList 的容量是此数组缓冲区的长度。 添加第一个元素时，任何带有elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA 的空 ArrayList 都将扩展为 DEFAULT_CAPACITY。
     */
    transient Object[] elementData; // 非私有，以简化嵌套类访问
```
#### 4.1.6、size

 ```java
   /**
     * ArrayList的大小（它包含的元素数）。
     */
    private int size;
```
#### 4.1.7、MAX_ARRAY_SIZE
   ```java
 /**
     * 要分配的最大数组大小。 尝试分配更大的数组可能会导致OutOfMemoryError：请求的数组大小超过VM限制
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
```
### 4.2、ArrayList 方法源码解析4.2、ArrayList 方法源码解析

#### 4.2.1、public void trimToSize()4.2.1、public void trimToSize()

将此ArrayList实例的容量调整为列表的当前大小。 应用程序可以使用此操作来最小化ArrayList实例的存储。
```java
public void trimToSize() {
        //列表结构修改次数加1
        modCount++;
        //如果列表实际大小 < elementData,调整ArrayList实例的容量调整为列表的当前大小（size）
        if (size < elementData.length) {
            elementData = (size == 0)
              ? EMPTY_ELEMENTDATA
              : Arrays.copyOf(elementData, size);
        }
    }
```
#### 4.2.2、public void ensureCapacity(int minCapacity)

如有必要，增加此ArrayList实例的容量，以确保它至少可以容纳由minCapacity参数指定的元素数。
```java
/**
     * 增加此ArrayList实例的容量
     *
     * @param   minCapacity   所需的最小容量
     */
    public void ensureCapacity(int minCapacity) {
        int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            ? 0
            // 大于默认值。 它被认为是默认大小。
            : DEFAULT_CAPACITY;
        //如果指定大小minCapacity 大于 minExpand，进行扩容
        if (minCapacity > minExpand) {
            ensureExplicitCapacity(minCapacity);
        }
    }
    
    private void ensureExplicitCapacity(int minCapacity) {
       //列表结构修改次数加1
        modCount++;

        // 如果指定大小大于当前列表数组缓冲区大小，则增加容量
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    
     private void grow(int minCapacity) {
        // 原有容量，当前数组缓存大小
        int oldCapacity = elementData.length;
        //新的容量=原有容量+（原有容量/2）
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        //如果新的容量 < minCapacity，设置新的容量大小为minCapacity
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        //如果新的容量 > MAX_ARRAY_SIZE
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    
     private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        //如果期望容量 minCapacity > MAX_ARRAY_SIZE（允许分配最大值），则设置大小为 Integer.MAX_VALUE
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

#### 4.2.3、public int size()

返回此列表中的元素数。

```java
public int size() {
        return size;
    }
```
#### 4.2.4、public boolean isEmpty()

如果此列表不包含任何元素，则返回true。

```java
public boolean isEmpty() {
        return size == 0;
    }
```
#### 4.2.5、public boolean contains(Object o)

如果此列表包含指定的元素，则返回true。当且仅当此列表包含至少一个元素e时才返回true（o == null？e == null：o.equals（e））。
```java
public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
```
#### 4.2.6、public int indexOf(Object o)

返回此列表中第一次出现的指定元素的索引，如果此列表不包含该元素，则返回-1。
 ```java
public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
```
#### 4.2.7、public int lastIndexOf(Object o)

返回此列表中指定元素最后一次出现的索引，如果此列表不包含该元素，则返回-1。
 ```java
public int lastIndexOf(Object o) {
        if (o == null) {
            for (int i = size-1; i >= 0; i--)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = size-1; i >= 0; i--)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
```
#### 4.2.8、public Object clone()4.2.8、public Object clone()

返回此ArrayList实例的副本。（元素本身不会被复制。）
```java
public Object clone() {
        try {
            ArrayList<?> v = (ArrayList<?>) super.clone();
            v.elementData = Arrays.copyOf(elementData, size);
            v.modCount = 0;
            return v;
        } catch (CloneNotSupportedException e) {
            // this shouldn't happen, since we are Cloneable
            throw new InternalError(e);
        }
    }
```
#### 4.2.9、public Object[] toArray()4.2.9、public Object[] toArray()

以适当的顺序（从第一个元素到最后一个元素）返回包含此列表中所有元素的数组。返回的数组将是“安全的”，因为此列表不会保留对它的引用。 （换句话说，此方法必须分配一个新数组）。 因此调用者可以自由修改返回的数组。

 ```java
public Object[] toArray() {
        return Arrays.copyOf(elementData, size);
    }
```
#### 4.2.10、public T[] toArray(T[] a)4.2.10、public T[] toArray(T[] a)

以适当的顺序返回包含此列表中所有元素的数组（从第一个元素到最后一个元素）; 返回数组的类型是指定数组的运行时类型。
 ```java
public <T> T[] toArray(T[] a) {
        if (a.length < size)
            // 根据给定数组的运行时类型创建一个新的数组
            return (T[]) Arrays.copyOf(elementData, size, a.getClass());
        System.arraycopy(elementData, 0, a, 0, size);
        //如果指定数组长度大于当前数组长度，则指定数组在 size 位置之后的元素设置为null
        if (a.length > size)
            a[size] = null;
        return a;
    }
```
#### 4.2.11、public E get(int index)4.2.11、public E get(int index)

返回此列表中指定位置的元素。
 ```java
public E get(int index) {
        rangeCheck(index);

        return elementData(index);
    }
//检查给定索引是否在范围内。 如果不是，则抛出适当的运行时异常。 此方法不检查索引是否为负数：如果索引为负，则抛出ArrayIndexOutOfBoundsException。
 private void rangeCheck(int index) {
        if (index >= size)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
    //根据给定的索引获取数组elementData中对应位置的元素
     E elementData(int index) {
        return (E) elementData[index];
    }
```
#### 4.2.12、public E set(int index, E element)4.2.12、public E set(int index, E element)

用指定的元素替换此列表中指定位置的元素。

```java
 public E set(int index, E element) {
        rangeCheck(index);

        E oldValue = elementData(index);
        elementData[index] = element;
        return oldValue;
    }
```
#### 4.2.13、public boolean add(E e)

将指定的元素追加到此列表的末尾。

```java
 public boolean add(E e) {
 		//扩大容量,修改modcount
        ensureCapacityInternal(size + 1);  // Increments modCount!!
       //数组是从0开始的存元素的，而数组个数是从1开始计数的
       //这里是往第size个位置上存元素
       //再将元素个数加1
        elementData[size++] = e;
        return true;
    }
    
private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }

private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
//计算数组容量
private static int calculateCapacity(Object[] elementData, int minCapacity) {
        //如果elementData为空，则设置容量大小为 Math.max(DEFAULT_CAPACITY, minCapacity);
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
```
#### 4.2.14、public void add(int index,E element)

将指定元素插入此列表中的指定位置。 将当前位于该位置的元素（如果有）和后续元素向右移动。

```java
 public void add(int index, E element) {
        //下标检查，是否越界了
        rangeCheckForAdd(index);
		 //扩增容量，同时改变modcount
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //index后面的元素后移
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
       //指定位置放置元素
        elementData[index] = element;
        //元素数量大小自增
        size++;
    }
```
#### 4.2.15、public E remove(int index)

删除此列表中指定位置的元素。 将任何后续元素向左移动）。

```java
 public E remove(int index) {
        rangeCheck(index);

        modCount++;
        //获取要删除的元素
        E oldValue = elementData(index);
        //需要移动的元素数量
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
```
#### 4.2.16、public boolean remove(Object o)

从此列表中删除指定元素的第一个匹配项（如果存在）。 如果列表不包含该元素，则不会更改。

```java
 public boolean remove(Object o) {
        if (o == null) {
            for (int index = 0; index < size; index++)
                if (elementData[index] == null) {
                    fastRemove(index);
                    return true;
                }
        } else {
             // 遍历ArrayList，找到元素o，删除并返回true。
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }
  //快速删除第index个元素并且不返回删除的值。
  private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work
    }
```
#### 4.2.17、public void clear()

从此列表中删除所有元素。 此调用返回后，列表将为空。

```java
 public void clear() {
        modCount++;

        // clear to let GC do its work
        for (int i = 0; i < size; i++)
            elementData[i] = null;

        size = 0;
    }
```
#### 4.2.18、public boolean addAll(Collection<? extends E> c)

将指定集合中的所有元素按指定集合的迭代器返回的顺序附加到此列表的末尾。 如果在操作正在进行时修改了指定的集合，则此操作的行为是不确定的。

```java
 public boolean addAll(Collection<? extends E> c) {
        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // Increments modCount
        System.arraycopy(a, 0, elementData, size, numNew);
        size += numNew;
        return numNew != 0;
    }
```
#### 4.2.19、public boolean addAll(int index, Collection<? extends E> c)

从指定位置开始，将指定集合中的所有元素插入此列表。 将当前位置的元素（如果有）和后续元素向右移动。 新元素将按照指定集合的迭代器返回的顺序出现在列表中。

```java
 public boolean addAll(int index, Collection<? extends E> c) {
        rangeCheckForAdd(index);

        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // Increments modCount

        int numMoved = size - index;
        if (numMoved > 0)
            System.arraycopy(elementData, index, elementData, index + numNew,
                             numMoved);

        System.arraycopy(a, 0, elementData, index, numNew);
        size += numNew;
        return numNew != 0;
    }
```
#### 4.2.20、protected void removeRange(int fromIndex,int toIndex)

从此列表中删除索引介于fromIndex（包含）和 toIndex（不包含）之间的所有元素。 将任何后续元素向左移动， 此调用通过（toIndex - fromIndex）元素缩短列表。如果toIndex == fromIndex，则此操作无效。

```java
protected void removeRange(int fromIndex, int toIndex) {
        modCount++;
        int numMoved = size - toIndex;
        System.arraycopy(elementData, toIndex, elementData, fromIndex,
                         numMoved);

        // clear to let GC do its work
        int newSize = size - (toIndex-fromIndex);
        for (int i = newSize; i < size; i++) {
            elementData[i] = null;
        }
        size = newSize;
    }
```
#### 4.2.21、public boolean removeAll(Collection<?> c)

从此列表中删除指定集合中包含的所有元素。

```java
public boolean removeAll(Collection<?> c) {
        Objects.requireNonNull(c);
        return batchRemove(c, false);
    }
//批量删除元素
private boolean batchRemove(Collection<?> c, boolean complement) {
        final Object[] elementData = this.elementData;
        int r = 0, w = 0;
        boolean modified = false;
        try {
            for (; r < size; r++)
                if (c.contains(elementData[r]) == complement)
                    elementData[w++] = elementData[r];
        } finally {
            // 保留与AbstractCollection的行为兼容性，即使c.contains（）抛出异常。
            if (r != size) {
                System.arraycopy(elementData, r,
                                 elementData, w,
                                 size - r);
                w += size - r;
            }
            if (w != size) {
                // clear to let GC do its work
                for (int i = w; i < size; i++)
                    elementData[i] = null;
                modCount += size - w;
                size = w;
                modified = true;
            }
        }
        return modified;
    }
```
#### 4.2.22、public boolean retainAll(Collection<?> c)：

从该列表中删除未包含在指定集合中的所有元素。

 ```java
public boolean retainAll(Collection<?> c) {
        Objects.requireNonNull(c);
        return batchRemove(c, true);
    }
```
#### 4.2.23、public ListIterator listIterator(int index)

从列表中的指定位置开始，返回列表中元素的列表迭代器。 指定的索引指示初始调用next时将返回的第一个元素。 对previous的初始调用将返回指定索引减去1的元素。
```java
 public ListIterator<E> listIterator(int index) {
        if (index < 0 || index > size)
            throw new IndexOutOfBoundsException("Index: "+index);
        return new ListItr(index);
    }
//AbstractList.ListItr 的优化版本
 private class ListItr extends Itr implements ListIterator<E> {
        ListItr(int index) {
            super();
            cursor = index;
        }

        public boolean hasPrevious() {
            return cursor != 0;
        }

        public int nextIndex() {
            return cursor;
        }

        public int previousIndex() {
            return cursor - 1;
        }

        @SuppressWarnings("unchecked")
        public E previous() {
            checkForComodification();
            int i = cursor - 1;
            if (i < 0)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i;
            return (E) elementData[lastRet = i];
        }

        public void set(E e) {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.set(lastRet, e);
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

        public void add(E e) {
            checkForComodification();

            try {
                int i = cursor;
                ArrayList.this.add(i, e);
                cursor = i + 1;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }
    }
```
#### 4.2.24、public ListIterator listIterator()

返回此列表中元素的列表迭代器（按适当顺序）。

```java
 public ListIterator<E> listIterator() {
        return new ListItr(0);
    }
```
#### 4.2.25、public Iterator iterator()

以适当的顺序返回此列表中元素的迭代器。

```java
 public Iterator<E> iterator() {
        return new Itr();
    }
 //AbstractList.Itr 的优化版本
 private class Itr implements Iterator<E> {
        int cursor;       // 要返回的下一个元素的索引
        int lastRet = -1; // 返回最后一个元素的索引; 如果没有返回 -1
        int expectedModCount = modCount;

        Itr() {}

        public boolean hasNext() {
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            //元素下标 >=size ,不存在对应位置的元素
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

        @Override
        @SuppressWarnings("unchecked")
        public void forEachRemaining(Consumer<? super E> consumer) {
            Objects.requireNonNull(consumer);
            final int size = ArrayList.this.size;
            int i = cursor;
            if (i >= size) {
                return;
            }
            final Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length) {
                throw new ConcurrentModificationException();
            }
            while (i != size && modCount == expectedModCount) {
                consumer.accept((E) elementData[i++]);
            }
            // 在迭代结束时更新一次以减少堆写入流量
            cursor = i;
            lastRet = i - 1;
            checkForComodification();
        }

        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }
```
#### 4.2.26、public List subList(int fromIndex,int toIndex)

返回指定fromIndex（包含）和toIndex（不包含）之间此列表部分的视图。如果fromIndex和toIndex相等，则返回的列表为空。 返回的列表由此列表支持，因此返回列表中的非结构更改将反映在此列表中，反之亦然。

 ```java
public List<E> subList(int fromIndex, int toIndex) {
        subListRangeCheck(fromIndex, toIndex, size);
        return new SubList(this, 0, fromIndex, toIndex);
    }

    static void subListRangeCheck(int fromIndex, int toIndex, int size) {
        if (fromIndex < 0)
            throw new IndexOutOfBoundsException("fromIndex = " + fromIndex);
        if (toIndex > size)
            throw new IndexOutOfBoundsException("toIndex = " + toIndex);
        if (fromIndex > toIndex)
            throw new IllegalArgumentException("fromIndex(" + fromIndex +
                                               ") > toIndex(" + toIndex + ")");
    }
    private class SubList extends AbstractList<E> implements RandomAccess {
        private final AbstractList<E> parent;
        private final int parentOffset;
        private final int offset;
        int size;

        SubList(AbstractList<E> parent,
                int offset, int fromIndex, int toIndex) {
            this.parent = parent;
            this.parentOffset = fromIndex;
            this.offset = offset + fromIndex;
            this.size = toIndex - fromIndex;
            this.modCount = ArrayList.this.modCount;
        }

        public E set(int index, E e) {
            rangeCheck(index);
            checkForComodification();
            E oldValue = ArrayList.this.elementData(offset + index);
            ArrayList.this.elementData[offset + index] = e;
            return oldValue;
        }

        public E get(int index) {
            rangeCheck(index);
            checkForComodification();
            return ArrayList.this.elementData(offset + index);
        }

        public int size() {
            checkForComodification();
            return this.size;
        }

        public void add(int index, E e) {
            rangeCheckForAdd(index);
            checkForComodification();
            parent.add(parentOffset + index, e);
            this.modCount = parent.modCount;
            this.size++;
        }

        public E remove(int index) {
            rangeCheck(index);
            checkForComodification();
            E result = parent.remove(parentOffset + index);
            this.modCount = parent.modCount;
            this.size--;
            return result;
        }

        protected void removeRange(int fromIndex, int toIndex) {
            checkForComodification();
            parent.removeRange(parentOffset + fromIndex,
                               parentOffset + toIndex);
            this.modCount = parent.modCount;
            this.size -= toIndex - fromIndex;
        }

        public boolean addAll(Collection<? extends E> c) {
            return addAll(this.size, c);
        }

        public boolean addAll(int index, Collection<? extends E> c) {
            rangeCheckForAdd(index);
            int cSize = c.size();
            if (cSize==0)
                return false;

            checkForComodification();
            parent.addAll(parentOffset + index, c);
            this.modCount = parent.modCount;
            this.size += cSize;
            return true;
        }

        public Iterator<E> iterator() {
            return listIterator();
        }

        public ListIterator<E> listIterator(final int index) {
            checkForComodification();
            rangeCheckForAdd(index);
            final int offset = this.offset;

            return new ListIterator<E>() {
                int cursor = index;
                int lastRet = -1;
                int expectedModCount = ArrayList.this.modCount;

                public boolean hasNext() {
                    return cursor != SubList.this.size;
                }

                @SuppressWarnings("unchecked")
                public E next() {
                    checkForComodification();
                    int i = cursor;
                    if (i >= SubList.this.size)
                        throw new NoSuchElementException();
                    Object[] elementData = ArrayList.this.elementData;
                    if (offset + i >= elementData.length)
                        throw new ConcurrentModificationException();
                    cursor = i + 1;
                    return (E) elementData[offset + (lastRet = i)];
                }

                public boolean hasPrevious() {
                    return cursor != 0;
                }

                @SuppressWarnings("unchecked")
                public E previous() {
                    checkForComodification();
                    int i = cursor - 1;
                    if (i < 0)
                        throw new NoSuchElementException();
                    Object[] elementData = ArrayList.this.elementData;
                    if (offset + i >= elementData.length)
                        throw new ConcurrentModificationException();
                    cursor = i;
                    return (E) elementData[offset + (lastRet = i)];
                }

                @SuppressWarnings("unchecked")
                public void forEachRemaining(Consumer<? super E> consumer) {
                    Objects.requireNonNull(consumer);
                    final int size = SubList.this.size;
                    int i = cursor;
                    if (i >= size) {
                        return;
                    }
                    final Object[] elementData = ArrayList.this.elementData;
                    if (offset + i >= elementData.length) {
                        throw new ConcurrentModificationException();
                    }
                    while (i != size && modCount == expectedModCount) {
                        consumer.accept((E) elementData[offset + (i++)]);
                    }
                    // update once at end of iteration to reduce heap write traffic
                    lastRet = cursor = i;
                    checkForComodification();
                }

                public int nextIndex() {
                    return cursor;
                }

                public int previousIndex() {
                    return cursor - 1;
                }

                public void remove() {
                    if (lastRet < 0)
                        throw new IllegalStateException();
                    checkForComodification();

                    try {
                        SubList.this.remove(lastRet);
                        cursor = lastRet;
                        lastRet = -1;
                        expectedModCount = ArrayList.this.modCount;
                    } catch (IndexOutOfBoundsException ex) {
                        throw new ConcurrentModificationException();
                    }
                }

                public void set(E e) {
                    if (lastRet < 0)
                        throw new IllegalStateException();
                    checkForComodification();

                    try {
                        ArrayList.this.set(offset + lastRet, e);
                    } catch (IndexOutOfBoundsException ex) {
                        throw new ConcurrentModificationException();
                    }
                }

                public void add(E e) {
                    checkForComodification();

                    try {
                        int i = cursor;
                        SubList.this.add(i, e);
                        cursor = i + 1;
                        lastRet = -1;
                        expectedModCount = ArrayList.this.modCount;
                    } catch (IndexOutOfBoundsException ex) {
                        throw new ConcurrentModificationException();
                    }
                }

                final void checkForComodification() {
                    if (expectedModCount != ArrayList.this.modCount)
                        throw new ConcurrentModificationException();
                }
            };
        }

        public List<E> subList(int fromIndex, int toIndex) {
            subListRangeCheck(fromIndex, toIndex, size);
            return new SubList(this, offset, fromIndex, toIndex);
        }

        private void rangeCheck(int index) {
            if (index < 0 || index >= this.size)
                throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
        }

        private void rangeCheckForAdd(int index) {
            if (index < 0 || index > this.size)
                throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
        }

        private String outOfBoundsMsg(int index) {
            return "Index: "+index+", Size: "+this.size;
        }

        private void checkForComodification() {
            if (ArrayList.this.modCount != this.modCount)
                throw new ConcurrentModificationException();
        }

        public Spliterator<E> spliterator() {
            checkForComodification();
            return new ArrayListSpliterator<E>(ArrayList.this, offset,
                                               offset + this.size, this.modCount);
        }
    }
```
#### 4.2.27、public void forEach(Consumer<? super E> action)

对Iterable的每个元素执行给定操作，直到处理完所有元素或操作抛出异常为止。 除非实现类另有指定，否则操作按迭代顺序执行。 操作抛出的异常会转发给调用者。

```java
 @Override
    public void forEach(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        final int expectedModCount = modCount;
        @SuppressWarnings("unchecked")
        final E[] elementData = (E[]) this.elementData;
        final int size = this.size;
        //遍历元素，对每个元素执行指定操作
        for (int i=0; modCount == expectedModCount && i < size; i++) {
            action.accept(elementData[i]);
        }
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
    }
```
#### 4.2.28、public Spliterator spliterator()

在此列表中的元素上创建一个late-binding和fail-fast 的Spliterator。

   ```java
 @Override
    public Spliterator<E> spliterator() {
        return new ArrayListSpliterator<>(this, 0, -1, 0);
    }

    /** 基于索引的二分法，懒惰初始化的Spliterator */
    static final class ArrayListSpliterator<E> implements Spliterator<E> {

        private final ArrayList<E> list;
        private int index; // 当前索引
        private int fence; // 最后一个索引
        private int expectedModCount; // initialized when fence set

        /** 创建覆盖给定范围的新spliterator  */
        ArrayListSpliterator(ArrayList<E> list, int origin, int fence,
                             int expectedModCount) {
            this.list = list; // OK if null unless traversed
            this.index = origin;
            this.fence = fence;
            this.expectedModCount = expectedModCount;
        }

        private int getFence() { // 首次使用时将fence初始化为size
            int hi; // (a specialized variant appears in method forEach)
            ArrayList<E> lst;
            if ((hi = fence) < 0) {
                if ((lst = list) == null)
                    hi = fence = 0;
                else {
                    expectedModCount = lst.modCount;
                    hi = fence = lst.size;
                }
            }
            return hi;
        }

        public ArrayListSpliterator<E> trySplit() {
            int hi = getFence(), lo = index, mid = (lo + hi) >>> 1;
            return (lo >= mid) ? null : // 除非太小，否则将范围分成两半
                new ArrayListSpliterator<E>(list, lo, index = mid,
                                            expectedModCount);
        }

        public boolean tryAdvance(Consumer<? super E> action) {
            if (action == null)
                throw new NullPointerException();
            int hi = getFence(), i = index;
            if (i < hi) {
                index = i + 1;
                @SuppressWarnings("unchecked") E e = (E)list.elementData[i];
                action.accept(e);
                if (list.modCount != expectedModCount)
                    throw new ConcurrentModificationException();
                return true;
            }
            return false;
        }

        public void forEachRemaining(Consumer<? super E> action) {
            int i, hi, mc; // hoist accesses and checks from loop
            ArrayList<E> lst; Object[] a;
            if (action == null)
                throw new NullPointerException();
            if ((lst = list) != null && (a = lst.elementData) != null) {
                if ((hi = fence) < 0) {
                    mc = lst.modCount;
                    hi = lst.size;
                }
                else
                    mc = expectedModCount;
                if ((i = index) >= 0 && (index = hi) <= a.length) {
                    for (; i < hi; ++i) {
                        @SuppressWarnings("unchecked") E e = (E) a[i];
                        action.accept(e);
                    }
                    if (lst.modCount == mc)
                        return;
                }
            }
            throw new ConcurrentModificationException();
        }

        public long estimateSize() {
            return (long) (getFence() - index);
        }

        public int characteristics() {
            return Spliterator.ORDERED | Spliterator.SIZED | Spliterator.SUBSIZED;
        }
    }
```
#### 4.2.29、public boolean removeIf(Predicate<? super E> filter)

删除此集合中满足给定谓词的所有元素。 在迭代期间或通过谓词抛出的错误或运行时异常将返回给调用者。

  ```java
  @Override
    public boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        // 确定要删除哪些元素在此阶段从过滤谓词抛出的任何异常都将使集合保持不变
        int removeCount = 0;
        final BitSet removeSet = new BitSet(size);
        final int expectedModCount = modCount;
        final int size = this.size;
        for (int i=0; modCount == expectedModCount && i < size; i++) {
            @SuppressWarnings("unchecked")
            final E element = (E) elementData[i];
            if (filter.test(element)) {
                removeSet.set(i);
                removeCount++;
            }
        }
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }

        // 移动幸存元素
        final boolean anyToRemove = removeCount > 0;
        if (anyToRemove) {
            final int newSize = size - removeCount;
            for (int i=0, j=0; (i < size) && (j < newSize); i++, j++) {
                i = removeSet.nextClearBit(i);
                elementData[j] = elementData[i];
            }
            for (int k=newSize; k < size; k++) {
                elementData[k] = null;  // Let gc do its work
            }
            this.size = newSize;
            if (modCount != expectedModCount) {
                throw new ConcurrentModificationException();
            }
            modCount++;
        }

        return anyToRemove;
    }
```
#### 4.2.30、public void replaceAll(UnaryOperator operator)

将该列表的每个元素替换为将运算符应用于该元素的结果。 操作抛出的错误或运行时异常将转发给调用者。

```java
 @Override
    @SuppressWarnings("unchecked")
    public void replaceAll(UnaryOperator<E> operator) {
        Objects.requireNonNull(operator);
        final int expectedModCount = modCount;
        final int size = this.size;
        //遍历元素，为每个元素应用operator
        for (int i=0; modCount == expectedModCount && i < size; i++) {
            elementData[i] = operator.apply((E) elementData[i]);
        }
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
        modCount++;
    }
```
#### 4.2.31、public void sort(Comparator<? super E> c)

根据指定的比较器对此列表进行排序。

此列表中的所有元素必须使用指定的比较器进行相互比较（即，c.compare（e1，e2）不得为列表中的任何元素e1和e2抛出ClassCastException）。

如果指定的比较器为null，则此列表中的所有元素都必须实现Comparable接口，并且应使用元素的自然顺序。

此列表必须是可修改的，但无需调整大小。

```java
 @Override
    @SuppressWarnings("unchecked")
    public void sort(Comparator<? super E> c) {
        final int expectedModCount = modCount;
        Arrays.sort((E[]) elementData, 0, size, c);
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
        modCount++;
    }
```
### 4.3、ArrayList 方法总结

1）ArrayList 实际上是通过一个数组去保存数据的。当我们构造ArrayList时；若使用默认构造函数，则ArrayList的默认容量大小是10。

2）当ArrayList容量不足以容纳全部元素时，ArrayList会重新设置容量：新的容量=原有容量+（原有容量/2）。

3）ArrayList的克隆函数，即是将全部元素克隆到一个数组中。

4）ArrayList实现java.io.Serializable的方式。当写入到输出流时，先写入“容量”，再依次写入“每一个元素”；当读出输入流时，先读取“容量”，再依次读取“每一个元素”。

5）ArrayList基于数组实现，可以通过下标索引直接查找到指定位置的元素，因此查找效率高，但每次插入或删除元素，就要大量地移动元素，插入删除元素的效率低。

6）在查找给定元素索引值等的方法中，源码都将该元素的值分为null和不为null两种情况处理，ArrayList中允许元素为null。

## 5、ArrayList toArray() 异常

当我们调用ArrayList中的 toArray()，可能会遇到抛出“java.lang.ClassCastException”异常的情况。

ArrayList提供了2个toArray()函数：

```java
public Object[] toArray() 
public <T> T[] toArray(T[] a) 
```
调用 toArray() 函数会抛出“java.lang.ClassCastException”异常，但是调用 toArray(T[] contents) 能正常返回 T[]。

toArray() 会抛出异常是因为 toArray() 返回的是 Object[] 数组，将 Object[] 转换为其它类型(例如，将Object[]转换为的Integer[])则会抛出“java.lang.ClassCastException”异常，因为Java不支持向下转型。

解决该问题的办法是调用 T[] toArray(T[] contents) ， 而不是 Object[] toArray()。

调用 toArray(T[] contents) 返回T[]的可以通过以下几种方式实现。

```java
// toArray(T[] contents) 方式1
public static Integer[] toArrayMethod1(ArrayList<Integer> v) {
    Integer[] newText = new Integer[v.size()];
    v.toArray(newText);
    return newText;
}

// toArray(T[] contents) 方式2
public static Integer[] toArrayMethod2(ArrayList<Integer> v) {
    Integer[] newText = (Integer[])v.toArray(new Integer[0]);
    return newText;
}

// toArray(T[] contents) 方式3
public static Integer[] toArrayMethod3(ArrayList<Integer> v) {
    Integer[] newText = new Integer[v.size()];
    Integer[] newText2 = (Integer[])v.toArray(newText);
    return newText2;
}
```
## 6、ArrayList 遍历方式

### 6.1、通过迭代器（Iterator）遍历
```java
List<String> list = new ArrayList<>();
Iterator iter = list.iterator();
while (iter.hasNext()) {
    System.out.println(iter.next());
}
```
### 6.2、通过索引遍历
由于ArrayList实现了RandomAccess接口，它支持通过索引值去随机访问元素。
```java
 for (int i = 0; i < list.size(); i++) {
        System.out.println(list.get(i));
    }
```
### 6.3、for循环遍历
 ```java
for (String s:list){
        System.out.println(s);
    }
```
## 7、ArrayList 扩容策略

ArrayList底层是使用**数组存储**的，当数组**大小不足存放新增元素的时候，才会发生扩容**。

在add操作中，ArrayList首先会调用ensureCapacityInternal方法进行**扩容**检测的。

如果数组大小不足，则会**自动扩容**；如果扩容后的大小**超出**数组最大的大小，则会**抛出异常**。

ArrayList扩容策略，主要有**两个**步骤：

🌂扩容检测（ensureCapacityInternal(size + 1)）：
检测数组大小是否为0，如果是，则使用默认的扩容大小10
检测是否需要扩容，只有当数组期望容量大于当前数组大小时，才会进行扩容

🌂扩容操作 grow和hugeCapacity
进行数组越界判断
拷贝原始数据到新的数组中
```java
//扩容检测
     private static int calculateCapacity(Object[] elementData, int minCapacity) {
	 
       // 如果底层数组大小为0，则使用默认的容量大小10
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }

    private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }

   // 数据结构发生改变，和fail-fast机制有关，在使用迭代器过程中，只能通过迭代器的方法（比如迭代器中add，remove等），修改List的数据结构，
    // 如果使用List的方法（比如List中的add，remove等），修改List的数据结构，会抛出ConcurrentModificationException
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

       // 当前数组容量大小不足时，才会调用grow方法，自动扩容
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    //扩容操作 grow和hugeCapacity
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        // 新的容量大小（即原容量1.5倍） = 原容量大小+（原容量大小/2）
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```
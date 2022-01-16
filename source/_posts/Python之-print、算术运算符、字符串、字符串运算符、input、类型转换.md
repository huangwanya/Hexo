---
title: Python之----print、算术运算符、字符串、字符串运算符、input、类型转换
date: 2020-02-05 13:23:35
tags: python
categories: python基础
---
## 第一个程序：
##### Hello world
```python
print('Hwllo world')
print("Hello world")
```
######  输出结果：
    Hwllo world
    Hello world
🌂：另外在python当中，单引号与双引号是一样的意思，都是表示字符串；
 
#####  print
在Python中使用print 函数产生输出。这会在屏幕上显示某些内容的文本表示形式。
```python
print('h\nw')
print(519+1)
```
输出结果：
```
h
w
520
```
##### Python中的算术运算符
首先±/*这些基本运算以及求模%便不必多讲，python中如果使用幂运算就要使用两个*号
`如：2**3 （2^3）`
我们假定两个不同的变量a=10，b=30，那么	b / a 输出结果 2
而取整除 - 返回商的整数部分（向下取整）

考虑以下代码：

    print(9 / 2)
    print(9 // 2)
  
    
最后输出结果分别为4.5与4

##### 字符串

我们可以使用引号('或")来创建字符串

    var1 = 'hello world'
    var2 = "hello world"

但是有一字符不能直接包含在字符串中。
例如，双引号不能直接包含在双引号字符串中；
此类字符必须通过在其前面加上反斜杠来转义。（\“）
🌂：Python提供了一种简便的方法，可以避免手动编写“ \ n”来对字符串中的换行符进行转义。用三组引号创建一个字符串，然后按Enter为您自动转义换行符。

```python
var1 = """ 有趣
是啊
嘻嘻"""
print(var1)
```
这个最终将输出
```
有趣
是啊
嘻嘻
```
##### 关于字符串的一些基本用法
```python
var1 = """I love Python"""
print(var1)
#下面是一些基本用法#
print("var1[0]:", var1[0])
print("var1[:6]:", var1[:6])
print("var1[6:]:", var1[6:])
print("var1[3:6]:", var1[3:6])

```
以下是输出结果:
```
I love Python
var1[0]: I
var1[:6]: I love
var1[6:]:  Python
var1[3:6]: ove
```
##### 字符串的拼接
```python
var1 = """I love Python"""
print('拼接结果：', var1+'我爱Python')
```
##### 字符串运算符

不喜欢定义，直接上代码，看着直观
```python
var1 = '学习'
var2 = 'Python'

print("var1 + var2 输出结果：", var1 + var2)
print("var1 * 2 输出结果：", var1 * 2)

if '学' in var1:
    print('学在变量var1中')
else:
    print('学不在变量var1中')

if '爱' not in var1:
    print('爱不在变量var1中')
else:
    print('爱在变量var1中')

```
```
var1 + var2 输出结果： 学习Python
var1 * 2 输出结果： 学习学习
学在变量var1中
爱不在变量var1中
```
##### input函数
提供了 input() 内置函数从标准输入读入一行文本，默认的标准输入是键盘。
```python
sc = input("Enter something please: ")
print(sc)
```
##### 类型转换（参考sololearn）

在Python中，由于涉及不同的类型而无法完成某些操作。
Eg：不能将包含数字2和3的两个字符串加在一起以产生整数 5，因为将对字符串执行运算，结果为’23’。
那么如何解决这个问题，答案是类型转换
考虑下面这个代码，并比较输出结果
```python
print("2"+"5")
print(2+5)
print(int("2")+int("5"))
print(int("2"+"5"))
```
结果为：
```
25
7
7
25

```



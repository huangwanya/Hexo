---
title: python之循环中的那些事儿
date: 2020-02-05 14:42:36
tags: python
categories: python基础
---
盘点python中的循环也就那么回事，废话不罗嗦，下面一起看看吧！
##### if 语句
Python中if语句的一般形式如下所示：
   ```python
 if condition_1:
        statement_block_1
    elif condition_2:
        statement_block_2
    else:
        statement_block_3
```
·如果 "condition_1" 为 True 将执行 "statement_block_1" 块语句
·如果 "condition_1" 为False，将判断 "condition_2"
·如果"condition_2" 为 True 将执行 "statement_block_2" 块语句
·如果 "condition_2" 为False，将执行"statement_block_3"块语句

Python 中用 elif 代替了 else if，所以if语句的关键字为：if – elif – else。

###### 注意：
1、每个条件后面要使用冒号 :，表示接下来是满足条件后要执行的语句块。
2、使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块。
3、在Python中没有switch – case语句。

###### 直接上代码：
```python
age=int(input("请输入狗的年龄："))
print(" ")
if  age<=0:
    print("你在逗我吧！")
elif age==1:
    print("想当人类14岁")
elif age==2:
    print("相当于人类22岁")
elif age>2:
    sui=22+(age-2)*5
    print("相当于人类",sui)
```
结果为：
```python
请输入狗的年龄：5
相当于人类 37
```
##### if 嵌套
在嵌套 if 语句中，可以把 if...elif...else 结构放在另外一个 if...elif...else 结构中。
```python
if 表达式1:
    语句
    if 表达式2:
        语句
    elif 表达式3:
        语句
    else:
        语句
elif 表达式4:
    语句
else:
    语句
```
下面看一个例子吧：
```python
# !/usr/bin/python3
 
num=int(input("输入一个数字："))
if num%2==0:
    if num%3==0:
        print ("你输入的数字可以整除 2 和 3")
    else:
        print ("你输入的数字可以整除 2，但不能整除 3")
else:
    if num%3==0:
        print ("你输入的数字可以整除 3，但不能整除 2")
    else:
        print  ("你输入的数字不能整除 2 和 3")
```
```python
输入一个数字：6
你输入的数字可以整除 2 和 3
```

##### while 循环
同样需要注意冒号和缩进。另外，在 Python 中没有 do..while 循环。
以下实例使用了 while 来计算 1 到 100 的总和：
```python
#!/usr/bin/env python3
 
n = 100
 
sum = 0
counter = 1
while counter <= n:
    sum = sum + counter
    counter += 1
 
print("1 到 %d 之和为: %d" % (n,sum))
```
执行结果为：
`1 到 100 之和为: 5050`
##### for 语句
```python
#!/usr/bin/python3
 
sites = ["Baidu", "Google","Runoob","Taobao"]
for site in sites:
    if site == "Runoob":
        print("HW!")
        break
    print("循环数据 " + site)
else:
    print("没有循环数据!")
print("完成循环!")
```
```python
循环数据 Baidu
循环数据 Google
HW!
完成循环!
```
进行循环嵌套原理一样，稍加理解即可。

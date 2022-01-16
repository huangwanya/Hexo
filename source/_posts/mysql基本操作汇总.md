---
title: mysql基本操作汇总
date: 2020-05-01 09:58:09
tags: MySQL
categories: 数据库
---
## 一、对数据库的操作

### 1. 创建一个库

create database 库名；

创建带有编码的：create database 库名 character set 编码；

查看编码：show create database 库名；

### 2. 删除一个库

drop database 库名；

### 3. 使用库

use 库名；

### 4.查看当前正在操作的库

select database();

## 二、对数据库表的操作

### 1.创建一张表

create table 表名(

字段名 类型(长度) [约束],

字段名 类型(长度) [约束],

字段名 类型(长度) [约束]

);

### 2.查看数据库表

创建完成后，我们可以查看数据库表

show tables;

**查看表的结构**：desc 表名

### 3.删除一张表

drop table 表名

### 4.修改表

#### 4.1 添加一列

alter table 表名 add 字段名 类型(长度) [约束]

#### 4.2 修改列的类型(长度、约束)

alter table 表名 modify 要修改的字段名 类型(长度) [约束]

#### 4.3 修改列的列名

alter table 表名 change 旧列名 新列名 类型(长度) [约束]

#### 4.4 删除表的列

alter table 表名 drop 列名

#### 4.5 修改表名

rename table 表名 to 新表名

#### 4.6 修改表的字符集

alter table 表名 character set 编码

#### 4.7查看当前表的编码

show create table 表名

## 三、对数据库表记录进行操作(重点)

### 1.插入记录

insert into 表名(列名1,列名2,列名3……) values(值1,值2,值3……)

insert into 表名 values(值1,值2,值3……)

### 2.修改表记录

#### 2.1 不带条件的

update 表名 set 字段名=值, 字段名=值, 字段名=值……(它会将该列的所有记录都更改)

#### 2.2 带条件的

update 表名 set字段名=值, 字段名=值, 字段名=值…… where 条件

### 3.删除表记录

#### 3.1带条件的

delete from 表名 where 条件

#### 3.2.不带条件的

先准备数据

delete from 表名;

#### **3.3 \**面试题\****

说说delete与truncate的区别？

delete删除的时候是一条一条的删除记录，它配合事务，可以将删除的数据找回。

truncate删除，它是将整个表摧毁，然后再创建一张一模一样的表。它删除的数据无法找回。

## 4.查询操作

语法：

select [distinct] *| 列名，列名 from 表名 [where条件]

#### 4.1 简单查询(以下表为例)

**product表：**

| pid  | pname | price | pdata |
| ---- | ----- | ----- | ----- |
| 1    | 关羽  | 10    | 2020  |
| 2    | 张飞  | 99    | 2021  |
| 3    | 刘备  | 56    | 2023  |



##### 1.查询所有商品

select * from product；


2. ##### 查询商品名和商品价格

select pname,price from product;

##### 3.查询所有商品信息使用表别名

select * from product as p;      (其中这个as可以省略)

##### 4.查询商品名，使用列别名

select pname as p from product

##### 5.去掉重复值(按照价格)

select **distinct**(price) from product

##### 6.将所有的商品的价格+10进行显示

select pname,price+10 from product;

#### 4.2 条件查询

##### 1.查询商品名称为"张飞"的商品信息

select * from product where pname='张飞';

##### 2.查询价格>60元的所有商品信息

select * from product where price>60;

##### 3.查询商品名称含有"羽"字的商品信息

select* from product where pname like‘%羽%’；

##### 4.查询商品id在(1，2，5)范围内的所有商品信息

select* from product where pid in（1，2，5）；

##### 5.查询商品名称含有"羽"字并且id为2的商品信息

select* from product where pname like‘%羽%’and pid=2；

##### 6.查询id为2或者6的商品信息

select* from product where pname like‘%羽%’or pid=2；

#### 4.3 排序 

##### 1.查询所有的商品，按价格进行排序(升序、降序)

**升序:** select* from product  order by price asc;

**降序:**elect* from product  order by price desc;

##### 2.查询名称有"羽"的商品信息并且按照价格降序排序

select* from product where pname like‘%羽%’order by price desc;

#### 4.4 聚合函数

##### 1.获得所有商品的**价格**的总和

select sum(price) from product;

##### 2.获得所有商品的平均价格

select avg(price) from product;

##### 3.获得所有商品的个数

select count(*) from product;

#### 4.5 分组操作

##### 1.添加分类id 

alter table product add cid varchar(32);

##### 2.初始化数据

update product set cid='1';

update product set cid='2' where  pid in (5,6,7);

1.根据cid字段分组，分组后统计商品的个数。

Select cid,count(*) from product group by cid;

2.根据cid分组，分组统计每组商品的平均价格，并且平均价格大于20000元。

select cid,avg(price) from product group by cid having avg(price)>>20000;

4.6 查询总结

select  一般在的后面的内容都是要查询的字段

from  要查询到表

where

group by

having  分组后带有条件只能使用having

order by 它必须放到最后面
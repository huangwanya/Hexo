---
title: mysql 常用命令整理
date: 2020-04-20 15:55:05
tags: MySQL
categories: 数据库
---

## 前言：

​         大二下终于开始学习数据库这门课程了，然而特别有意思的是竟然是在家上网课 ，哎，，，，，，这也是没办法呀，为国家做贡献嘛，本着好好学习，天天向上的态度，自己补一补上机的内容，毕竟这也没办法上机实践，于是乎开始学习基本的mysql常用命令，毕竟磨刀不误砍柴工，开干.....................

## 一、连接

因为还没尝试过远程，所以这里就用本地做实验，在已安装数据库的情况下有两种方法进入并且连接数据库。

(1)在cmd下直接运行 mysql -uroot -p    然后输入数据库密码就行

(2)直接打开MySQL输入密码即可

最后连接成功如下图：

![](https://s1.ax1x.com/2020/04/20/JliLXq.png)

## 二、退出命令

格式：exit 或 quit

ps:英语基础稍微好点儿的都知道这两个单词都是退出的意思！

**示例**

```mysql
//需要连接后使用，mysql>下执行
exit
```

## 三、数据库操作命令

```mysql
//查看数据库
show databases
//使用默认字符集和排序规则创建数据库
create database db_name; 
//创建数据库并指定字符集编码
create database if not exists db_name default character set = 'utf8';
//创建数据库并指定字符集编码和排序规则
create database db_name DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
//删除数据库
drop database db_name
//指定当前数据库
USE db1;
```

## 四、数据表操作命令

```mysql
//指定当前数据库
USE db1;
//查询表
show tables
//显示表结构
desc tb_name；
//创建表
create table Student(
 Sno varchar(10) primary key,
 Sname varchar(20) unique,
 Ssex varchar(2),
 Sage int,
 Sclass varchar(20)
)；
//删除表
 drop table tb_name;
```

## 五、增删改查表数据

```mysql
//插入数据
insert into Student values ('20180001','小明','男',20,'一班');
//按字段插入数据
insert into Student (Sno,Sname,Ssex,Sage,Sclass) values ('20180003','小红','男',18,'二班');
//删除记录
delete from Student where Sno='20180001'
//删除所有
delete * from Student
//更新
update Student set Sclass = '一班' where Sname='小红'
//查询
select * from Student; 
```

## 六、用户和权限管理

```mysql
//指定数据库
Use mysql;
//查询所有用户
Select user,host from user;
//创建本地访问用户
create user admin@'localhost' identified by '密码';
//创建允许远程访问用户
create user admin@'%' identified by '密码';
//授权所有权限
GRANT ALL PRIVILEGES ON *.* TO admin@"localhost";
//授权所有权限
GRANT ALL PRIVILEGES ON *.* TO admin@"%";
//修改密码 mysql 5.7-8.0
ALTER USER 'admin'@'localhost' IDENTIFIED BY '密码';
//修改密码 mysql 5.6
update user set password=password("你的新密码") where user="admin";
//修改密码 mysql 5.5
use mysql;
set password for admin@localhost = password('密码');
//刷新权限
flush privileges;
//删除用户
drop user admin@'%';
```

## 七、查看mysql常用参数

```mysql
//查询mysql版本
select @@version;
select version();
//查询进程列表
 show processlist;
//查看进程列表完整信息
show full processlist;
//查看线程数
show status like 'Threads%'; 
//查看连接数
show variables like '%max_connections%'; 
//查看数据默认字符集
show variables like '%character%';
//查看数据库默认排序规则
show variables like 'collation%';
//查看支持字符集
show charset;
```


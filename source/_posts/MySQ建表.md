---
title: MySQ建表
date: 2020-04-20 19:37:55
tags: MySQL
categories: 数据库
---
## 前言：

首先纵观数据库原理及应用书上主要围绕几张表格进行的。比如学生表呀.........这里就首先统一建立起来方便后面应用！

## 一·建立数据库

**命令：**

```MySQL
CREATE DATABASE 数据库名;
```

这里建立书上第一个需要用到的数据库命名为shangji1(名字随意)

## 二·建立表格

**注意：**建表之前一定记得选定表明（use 表名；)这里建立本学期常用的表格及附上代码，以便后期复习。

**Student：**

```MySQL
CREATE TABLE Student
(Sno CHAR(9)PRIMARY KEY,
 Sname CHAR(20) UNIQUE,
 Sex CHAR(2),
 Sage SMALLINT,
 Sdept CHAR(20)
);
```

**Course：**

```MySQL
CREATE TABLE Course
(Cno CHAR(4)PRIMARY KEY,
 Cname CHAR(40)NOT NULL,
 Cpno CHAR(4),
 Ccredit SMALLINT,
 FOREIGN KEY(Cpno)REFERENCES Course(Cno)
);
```

**SC：**

```MySQL
CREATE TABLE SC 
(Sno CHAR(9),
 Cno CHAR(4),
 Grade SMALLINT,
 PRIMARY KEY(Sno,Cno),
 FOREIGN KEY(Sno)REFERENCES Student(Sno),
 FOREIGN KEY(Cno)REFERENCES Course(Cno)
);
```

**S:**

```mysql
CREATE TABLE S
 (SNO CHAR(20)PRIMARY KEY,
  SNAME CHAR(20),
  STATUS CHAR(20),
  CITY CHAR(20)
);
```

**p:**

```mysql
CREATE TABLE P
 (PNO CHAR(20)PRIMARY KEY,
  PNAME CHAR(20),
  COLOR CHAR(20),
  WEIGHT CHAR(20)
);
```

**J:**

```mysql
CREATE TABLE IF NOT EXISTS P
 (PNO CHAR(20)PRIMARY KEY,
  PNAME CHAR(20),
  COLOR CHAR(20),
  WEIGHT CHAR(20)
);
```

**SPJ:**

```mysql
create table spj(
sno char(5),
pno char(5),
jno char(5),
qty int,
primary key(sno,pno,jno),
foreign key(sno) references s(sno),
foreign key(pno) references p(pno),
foreign key(jno) references j(jno)
);

```


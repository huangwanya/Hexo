---
title: mysql错误
date: 2020-03-17 13:56:50
tags: MySQL
categories: 数据库
---
## MySql 建表时遇到[Err] 1050 - Table 'users' already exists异常解决方法

#### 首先看一下出现这个的代码：
```sql
CREATE TABLE Course
(Cno CHAR(4)PRIMARY KEY,
 Cname CHAR(40)NOT NULL,
 Cpno CHAR(4),
 Ccredit SMALLINT,
 FOREIGN KEY(Cpno)REFERENCES Course(Cno)
);
```
点击运行之后会出现[Err] 1050 - Table 'users' already exists异常解决方法这个错误提示，对此应该怎么解决呢，很简单

```sql
CREATE TABLE IF NOT EXISTS Course
(Cno CHAR(4)PRIMARY KEY,
 Cname CHAR(40)NOT NULL,
 Cpno CHAR(4),
 Ccredit SMALLINT,
 FOREIGN KEY(Cpno)REFERENCES Course(Cno)
);
```
至此完美解决
## 分析：
错误原因：重复创建了表格。
---
title: MySql
date: 2021-10-15 00:00:00
type: Mysql
description: 'MySql表入门基础'
cover: https://s3.bmp.ovh/imgs/2021/12/b8dacb96c7f542f3.png
---

# MySQL #

本地登录：进入bin目录看到mysql.exe 后打开bin目录的命令行

输入： mysql -uroot -p123456

![1622363577463](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622363577463.png)

退出命令：exit

![1622363740431](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622363740431.png)

隐藏密码登录：mysql -uroot -p

![1622363897343](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622363897343.png)

## mysql常用命令 ##

查看mysql中有哪些命令 ： show databases; 

选择某个数据库：use test

创建一个数据库：create database newtest;

展示本地数据库：show databases;

![1622364340400](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622364340400.png)

查看表：show tables; 返回几个表

9、关于8L语句的分类?

soL语句有很多，最好进行分门别类，这样更容易记忆分为

数据查语言(凡是带有seet关键字的都是查语句) select。

 DME 数据操语言(凡是对表当中的数据进行增删改的都是DL)

 insert delete update insert增

 delete

 update改

这个主要是操作表中的数据data。

 DDL: 数据定义语言

凡是带有 create、drop、 alter的都是DDL

DDL主要操作的是表的结构。不是表中的数据

 create:新建，等同于增

drop:删除

 alter:修改

这个增改和不同，这个主要是对表结构进行操作

 TCL：是事务控制语

包括：事务提交：commit;

​           事务回滚：rollback;

DCL：是数据控制语言。

例如：授权grant，撤销权限revoke

导入表：source D:\mysqltool\document\bjpowernode.sql

查看表：

mysql> show tables;
+-------------------+
| Tables_in_newtest |
+-------------------+
| dept              |     dept：部门表
| emp               |   emp：员工表
| salgrade          |  salgrade：是工资等级表
+-------------------+
3 rows in set (0.00 sec)

### `简单查询` ###

查看表中看所有数据/字段： select * from 表；

（ /*/效率低 ，建议把所有字段写上）

![1622366942505](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622366942505.png)

查询指定个字段：select 指定 from 表；

![1622367837498](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622367837498.png)

查看表的结构：desc 表；（describe ）

![1622367259165](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622367259165.png)

查询列中起别名：select 字段 as 新别名 form 表;

![1622368187183](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622368187183.png)

注意事项：

- 所有sql语言分号结尾；sql语句不区分大小写
- 数据库中的字符串采用单引号 

![1622367582850](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1622367582850.png)

## 条件查询 ##

语法格式： select `字段1` ,`字段2` from `表名` where `条件`；

- = 等于  select empno,ename from emp where sal`=`3000; 

- <> / ！= 不等于 select empno,ename from emp where sal `!=/<>` 3000;

- ＞大于/＜小于：select sal,ename from emp where sal `</>/<=/>=` 3000;

- between...and... ：两值之间，等同于 >= and <=

  select sal,ename from emp where sal `>=` 3000 `and`   `<=`4000;

  select sal,ename from emp where sal `between` 3000 `and` 4000;(左小右大)

- and并且：select 字段 from 表 where  job='clerk' `and` sal < 2000;

- or或者：select 字段 from 表 where job='clerk' `or` job = 'salesman';

- and与or 合并用：

  `and和or同时出现，and优先级较高；or加小括号提升优先级；`

  select 字段 from 表 where  sal > 2000 `and` `(`deptno = 10 `or` deptno = 20`)`;

-  is nul1为nul( is not null不为空)

- in包含，相当于多个or( not in不在这个范围中)

  select 字段 from 表 where  sal `in` (800,5000) ; `找出800 与 5000；`

-  not nott可以取非，主要用在is或in中

- 1ike：称为模糊查询，支持%或下划线。`%匹配配任意多个字符`

  下划线：表示任意一个字符 (%是一个特殊的符号，_也是一个特殊符号)

  找出名字有o的：select 字段from 表 where  ename `like '%o%`';

  ''%n%'含义n的;    '%n'以n开头的；  'n%' 以n结尾的；

  '_A%'找出第二个字母是A的；

  如：select name from t_student where  name like `'%\_%';`

  （找出名字中有下划线的）

  ## 排序查询 ##

  正序：select ename,job,sal from emp `order by` sal;

  ：select ename,job,sal from emp order by sal asc，ename asc;

  （如果出现工资相同，则按名字字母排序）

  ：select ename,job,sal from emp order by 2;（按照第二列排）

  降序：select ename,job,sal from emp `order by` sal `desc;`

  select 字段l from 表 `where sal between 1250 and 3000 order by sal ;` (指定数字之间向正序排列)

  关键字排序：select ... from ... where ...order by

  ## 当行处理函数 ##

  转大写：select lower(字段) from 表;

  转小写：select upper(字段) from 表;

  substr：截取字符串，起始下标，截取的长度

  select 字段 from 表 where substr(ename,1,1)
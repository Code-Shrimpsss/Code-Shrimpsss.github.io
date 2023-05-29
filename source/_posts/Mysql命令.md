---
title: 'Mysql命令'
date: 2021-10-20 00:00:00
description: 'Mysql实用命令全注解'
tag: Mysql
cover: https://s3.bmp.ovh/imgs/2021/12/b8dacb96c7f542f3.png
---

# Mysql #

------

[基于前文没安装的可以差看我之前的文章]()

## 初始化 ##

###### win端 ######

```mysql
# --- 管理员命令行 ----
# 1. 启动mysql服务
net start mysql

# 2. 输入你的账号密码
mysql -uxxx -pxxxxx

# or

# --- 直接在文件目录开启 ---
# 输入你的账号密码
mysql -uxxx -pxxxxx
```

###### linux端 ######

```mysql
# 1. 进入顶层目录 /
cd /
# 2. 进入mysql目录
cd /etc/mysql
# 3. 输入你的账号密码
mysql -uxxx -pxxxxx
```

###### 启动与停止 ######

```mysql
# 启动mysql服务
net start mysql

# 停止mysql服务
net start mysql
```

`注意，如果是连接到另外的机器上，则需要加入一个参数-h机器IP`

## 基本操作 ##

注意 **mysql 语法** 后面必须跟着 `;`号

### 库操作 ###

###### 查看所有数据库 ######

```mysql
show databases;
```

###### 使用数据库 ######

```mysql
use 数据库名;
```

###### 查看当前使用的数据库 ######

```mysql
select database();
```

###### 创建数据库 ######

```mysql
# 意为：创建 数据库 xxx 字符集为uf8格式;
create database 数据库名 charset=utf8;
# 例如:
create database Mydb charset=utf8;
```

###### 删除数据库 ######

```mysql
drop 数据库名;
# or 
drop table 数据库名;
```

### 表操作 ###

#### 表基本操作 ####

###### 查看当前数据库中的所有表 ######

```mysql
show tables;
```

###### 查看表结构 ######

```mysql
desc 表名;
```

###### 创建表 ######

创建Mysql数据表需要有：`表名`，`表字段名`，`定义每个表字段`。

```mysql
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] [database_name] <table_name>
（
	<column_name> <data_type> [[not null]...]
）
```

**解注:**

| 语法          | 释意                                       |
| ------------- | ------------------------------------------ |
| TEMPORARY     | 创建临时表                                 |
| IF NOT EXISTS | 如果要创建的表已经存在，强制不显示错误消息 |
| database_name | 数据库名                                   |
| table_name    | 表面                                       |
| column_name   | 列名                                       |
| data_type     | 数据类型                                   |

**例子:**

```mysql
# 创建一个名为 student 的表
create table students(
    # ID列为无符号整型，该列值不可以为空，并不可以重复，而且自增。
    id int unsigned primary key auto_increment not null,
    # name列单个最大字符长度为20 默认为空
    name varchar(20) default '',
    # age列为无符号整型 单个范围为(0,255) 默认为0
    age tinyint unsigned default 0,
    # height列最多存储5位数字，小数位为2位。 范围为:（-999.99 , 999.99）
    height decimal(5,2), 
    # gender列里面的字段只能为男或女，若新添加的一行不为这两个数据则报错
    gender enum('男','女'),
    # 自己思考下面列意
    cls_id int unsigned default 0
)
```

######  修改表 - 添加列

```mysql
alter table 表名 add 列名 类型；
# 如:
alter table student add birthday datetime;
```

######  修改表 - 列重命名

```mysql
alter table 表名 change 原名 新名 类型及约束;
# 如:
alter table student change birthday birth datetime not null;
```

######  修改表 - 列不重命名

```mysql
alter table 表名 modify 列名 类型与约束;
# 如:
alter table student modify birthday date not null;
```

######  修改表 - 删除列

```mysql
alter table 表名 drop 列名;
# 如:
alter table student drop birthday;
```

######  删除表

```mysql
drop table 表名;
# 如:
drop table student;
```

###### 清空表中记录 ######

```mysql
delete * from 表名;
```

###### 查看表中记录 ######

```mysql
select * from 表名;
```

###### 查看表的创建语句 ######

```mysql
show create table 表名;
# 如:
show create table student;
```

#### 增删改查e ####

##### 查询 #####

###### 查看所有列 ######

```mysql
select * from 表名;
# 如:
select * from student;
```

###### 查询指定列 ######

```mysql
select 列1,列2 form 表名;
# 如:
select id,name from student;
```

##### 增加 #####

`语法格式: INSERT [INTO] tb_name [(col_name,...)] {VALUES | VALUE} ({expr | DEFAULT},...),(...),...;`

**说明：**主键列是自动增长，但是在全列插入时需要占位，通常使用0或者 default 或者 null 来占位，插入成功后以实际数据为准

###### 全列插入 - 值的顺序与表中字段的顺序对应 ######

```mysql
insert into 表名 values(...)
# 如:
insert into student values(0,'小黑')
```

###### 部分列插入 - 值的顺序与给出的列顺序对应 ######

```mysql
insert into 表名(列1,...) values(值1,...)
# 如:
insert into student(name,hometown,birthday) values('小黑','深林','2010-9-1')
```

###### 全列多行插入 - 值的顺序与给出的列顺序对应 ######

```mysql
insert into 表名(列1,...) values(值1,...),(值1,...)...;
# 如:
insert into student(name) values('小黑'),('小兔'),('小龙');
```

##### 修改 #####

`语法格式: UPDATE tbname SET col1={expr1|DEFAULT} [,col2={expr2|default}]...[where 条件判断]`

```mysql
# UPDATE 更新修改 || set 获取想要修改的列更改对应的值  || where 设定查询条件
update 表名 set 列1=值1, 列2=值2... where 条件;
# 如:
update student set gender=0,hometown='南京' where id=5;
```

#####  删除

`语法格式: DELETE FROM tbname [where 条件判断]`

###### 基本删除 ######

```mysql
delete from 表名 where 条件;
# 如:
delete from student where id=5;
```

######  逻辑删除

```mysql
update student set isdelete=1 where id=1;
```



### 数据类型 ###

MySQL 中定义数据字段的类型对你数据库的优化是非常重要的。

MySQL 支持多种类型，大致可以分为三类：`数值、日期/时间和字符串`(字符)类型。

整数类型：BIT、BOOL、TINY INT、SMALL INT、MEDIUM INT、 INT、 BIG INT

浮点数类型：FLOAT、DOUBLE、DECIMAL

字符串类型：CHAR、VARCHAR、TINY TEXT、TEXT、MEDIUM TEXT、LONGTEXT、TINY BLOB、BLOB、MEDIUM BLOB、LONG BLOB

日期类型：Date、DateTime、TimeStamp、Time、Year

其他数据类型：BINARY、VARBINARY、ENUM、SET、Geometry、Point、MultiPoint、LineString、MultiLineString、Polygon、GeometryCollection等

[点击可查看具体参考博文](https://www.cnblogs.com/-xlp/p/8617760.html)

[菜鸟Mysql数据类型](https://www.runoob.com/mysql/mysql-data-types.html)
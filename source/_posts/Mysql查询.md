---
title: 'Mysql查询'
date: 2021-10-13 00:00:00
description: 'Mysql查询命令简手册'
tag: Mysql
cover: https://s3.bmp.ovh/imgs/2021/12/b8dacb96c7f542f3.png
---

# Mysql 查询

---

### 基本查询

-   查询所有字段

```mysql
select * from 表名;
```

-   查询指定路段

```mysql
select 列1,列2,... from 表名;
# 如:
select name from students;
```

-   起别名

```mysql
-- 如果是单表查询 可以省略表明
select id, name, gender from students;

-- 表名.字段名
select students.id,students.name,students.gender from students;

-- 可以通过 as 给表起别名
select s.id,s.name,s.gender from students as s;
```

-   消除重复行(去重)

```mysql
select distinct 列1,... from 表名;
# 如:
select distinct name from studetns;
```

### 运算优先级

![image.png](https://i.loli.net/2021/11/18/UzQOslGnpMHLKWC.png)

### 条件

##### **使用 where 子句对表中的数据筛选，并将筛选结果为符合的输出。**

> where 支持多种运算符进行条件处理，如:
>
> 比较运算符 逻辑运算符 模糊查询 范围查询 空判断
>
> **优先级由高到低的顺序为：小括号，not，比较运算符，逻辑运算符**

##### **比较运算符**

`等于: =` || `大于: >` || `大于等于: >=` || `小于: <` || `小于等于: <=` || `不等于: != 或 <>`

**-- 基本语法 --**

`SELECT 字段 FROM 表 WHERE 某字段 比较运算符 条件`

-   查询大于

```mysql
select * from student where id > 3;
```

-   查询不等于

```mysql
 select * from student where id != 3;
 # or
 select * from student where id <> 3;
```

-   查询姓名

```mysql
select * from student where name = '罗小黑';
```

-   查询未被删除

```mysql
# is_delete=1未被删除 =0则为已删除
select * from student where is_delete=1;
```

##### **逻辑运算符**

`and` || `or` || `not`

**-- 基本语法 --**

`SELECT 字段 FROM 表 WHERE 条件 逻辑运算符 条件`

-   查询版本是 AFC8 类型为华为的电脑

```mysql
select * from conputers where acg = 'AFC8' and types = '华为';
```

-   查询版本是 AFC8 的电脑 或 类型为华为的电脑

```mysql
select * from conputers where acg = 'AFC8' or types = '华为';
```

-   查询类型不为苹果的电脑

```mysql
select * from conputers where not types = '苹果';
```

##### **模糊查询**

**使用 like 搭配通配符**

`'_' ：`表示匹配任意一个字符,常用与充当占位符.

`'%'：`表示匹配 0 个或多个任意字符.

`[ ] ：`表示括号内所列字符中的一个（类似正则表达式).

`[^ ] ：`表示不在括号所列之内的单个字符。

`escape：` 取消%或\_字符的通配符特性

**-- 基本语法 --**
`SELECT 字段 FROM 表 WHERE 某字段 Like 条件`

-   使用'\_'

```mysql
 # 查询姓名为朱开头两个字的数据
 select * from students where name like '朱_';
```

-   使用'%'

```mysql
  # 查询姓名为尘结尾的数据
 select * from students where name like '%尘';
```

-   使用 escape

```mysql
 # 查询姓名中含有'%'的数据
 select * from students where name like '%P%%' escape 'P';
```

##### **范围查询**

`in` ：表示查询在一个非连续的范围内

`between...and...` ：表示查询在一个连续的范围内

**-- 基本语法 --**

in：`  SELECT 字段 FROM 表 WHERE 某字段 in(value1,value2...)`

between...and...：`SELECT 字段 FROM 表 WHERE 某字段 between value1 and value2`

-   查询编号是 1 或 3 或 8 的学生

```mysql
select * from students where id in(1,3,8);
```

-   查询编号为 3 至 8 的学生

    _between 可以连接多个 and，即可以添加多个判断条件_

```mysql
 select * from students where id between 3 and 8;
```

-   搭配操作

```mysql
select * from WST where (AID between 1 and 20) and areas not in ('USA', 'CNA');
```

-   小技巧

    _and 比 or 的优先级高，如果同时出现并希望先运算 or，可以运用()小括号使用。_

```mysql
select * from WST where (AID between 1 and 20) or areas = 'CNA';
```

##### **空判断**

`null 为空` **->** `is null判空` || `is not null 判非空`

**-- 基本语法 --**

`SELECT 字段 FROM 表 WHERE 某字段 is null `

-   查询上午没有签到的学生

```mysql
select * from conputers where openpick is null;
```

-   查询上午签到的学生

```mysql
select * from conputers where openpick is not null;
```

### 排序

使用 **`ORDER BY`** 子句来设定你想按哪个字段哪种方式来进行排序 ，再返回搜索结果

**在指定 `ORDER BY` 子句时，要把它放在最后（即最后一条子句）**

使用 `ASC` 或 `DESC` 关键字来设置查询结果是按**升序**或**降序**排列 ，默认升序[DESC]

`DESC`是`DESCENDING`的缩写，这两个关键字都可以使用

**-- 基本语法 --**

**原：**`SELECT 字段 FROM 表 ORDER BY 某字段; `

**搭配关键字：**`SELECT 字段 FROM 表 ORDER BY 某字段 [ASC [DESC][默认 ASC]]; `

-   排序查询学生 id 与姓名

```mysql
select sid,sname from students order by sid,sname
```

-   按指定列进行排序

```mysql
# 这里指的是先按3即sage排序
select sname,sheight,sage from students order by 3,2;
```

-   降序查询分数及格的学生

```mysql
select * from students where fration > 60 order by id desc;
```

-   查询所有学生，先按年龄排序，后按身高排序

```mysql
select * from students order by age desc,height desc;
```

### 聚合函数

**聚合函数在数据库数据的查询分析中，应用十分广泛**

-   AVG() - 平均函数

```mysql
语法: AVG([DISTINCT] expr)
返回值: 返回expr的值
取反: DISTINCT为返回expr的不同值的平均值
如果没有匹配的行，AVG()返回null。
定义: 表示求此列的平均值
```

-   MAX() / MIN() - 最大/ 最小函数

```mysql
语法：select max(age)/min(age) from;
定义: 求此列的最大或最小值
```

-   SUM() - 求和函数

```mysql
语法：select sum(age) from student;
定义: 求此列的和
```

-   COUNT() - 总数函数

```mysql
语法：select count(*) from student;
定义: count(*)表示计算总行数，括号中写星与列名，结果是相同的
```

-   CONCAT() - 合并函数

```mysql
语法：CONCAT(sal * 12 + 200);
定义: 拼接数据
```

### 分组

#### **Group By**

`将查询结果按照1个或多个字段进行分组，字段值相同的为一组`

```mysql
SELECT column_name, function(column_name)
FROM table_name
GROUP BY column_name;
```

##### **group by + group_concat()**

表示分组之后，根据分组结果，使用 group_concat(**字段**)来放置每一组的某字段的值的集合

```mysql
select gender,group_concat(id) from students group by gender;

'''
+--------+------------------+
| gender | group_concat(id) |
+--------+------------------+
| 男     | 3,4,8,9,14       |
| 女     | 1,2,5,7,10,12,13 |
+--------+------------------+
'''
```

##### **group by + 集合函数**

可以通过集合函数来对这个`分组完值的集合`做一些操作

```mysql
select gender,count(*) from students group by gender;

'''
+--------+----------+
| gender | count(*) |
+--------+----------+
| 男     |        5 |
| 女     |        7 |
+--------+----------+
'''
```

##### **group by + with rollup**

在最后新增一行，来记录**当前列里所有记录的总和**

```mysql
select gender,count(*) from students group by gender with rollup;

'''
+--------+----------+
| gender | count(*) |
+--------+----------+
| 男     |        5 |
| 女     |        7 |
| NULL   |       12 |
+--------+----------+
'''
```

##### group by + having\*\*

1. having 条件表达式：用来分组查询后指定一些条件来输出查询结果
2. having 作用和 where 一样，但`having只能用于group by`

```mysql
select gender,count(*) from students group by gender having count(*)<7;

'''
+--------+----------+
| gender | count(*) |
+--------+----------+
| 男     |        5 |
+--------+----------+
'''
```

**`having使用规范`**

**1.having 只能用在 group by 之后，对分组后的结果进行筛选(即使用 having 的前提条件是分组)。**

**2.where 肯定在 group by 之前。**

**3.where 后的条件表达式里不允许使用聚合函数，而 having 可以。**

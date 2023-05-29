---
title: 'Mysql创建账号时丢失或无法创建解决'
date: 2021-12-14 00:17:00
description: 'Mysql创建账号时丢失或无法创建解决'
tag: Mysql
cover: https://s3.bmp.ovh/imgs/2021/12/b8dacb96c7f542f3.png
---

{% note pink 'fas fa-car-crash' simple %}
本教程只适合Mysql账号遇到的问题
{% endnote %}

### 场景一：创建账号后 原账户丢失 ###

##### 第一步：设置允许无密码登录，重启mysql #####

```python
hadoop@ycm:~$ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
hadoop@ycm:~$service mysql restart
```

**在[mysqld]中添加`skip-grant-tables`**

##### 第二步：进入mysql交互模式 #####

```python
mysql -u root -p
```

**然后直接回车就行**

##### 第三步： #####

```python
mysql> use mysql;``mysql>flush ``privileges``;``mysql>``UPDATE` `user` `SET` `authentication_string=``""` `WHERE` `user``=``"root"``;//先把root密码置为空``mysql>flush ``privileges``;``mysql>``ALTER` `user` `'root'``@``'localhost'` `IDENTIFIED ``BY` `'Ycm@123nihao'``;//再重置密码
```

注意：密码格式必须符合要求，不然会报错的。mysql8貌似是要求必须包括大小写,数字和特殊字符。

##### 第四步：quit退出mysql交互模式，去掉之前加的skip-grant-tables，再重启mysql #####

##### 第五步：这回mysql -u root -p输入设置的正确密码就能进入mysql了 #####



> 参考文章：[[永远的小幸运]](https://www.cnblogs.com/y-c-m520/p/14060382.html)


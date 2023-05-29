---
title: Redis - Python交互
date: 2021-7-11 00:08:00
description: '在Python上操控Redis'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

### 先导 ###

#### Python导包 ####

进⼊虚拟环境py_django，联⽹安装包redis

> `pip install redis`
>
> **如果pip不成功则用下面easy**
>
> `easy_install redis`

#### **文件中调用模块** ####

引⼊模块

> `from redis import *`
>
> 本章主要用的是 redis模块中的 `StrictRedis对象`

### StrictRedis对象⽅法 ###

**通过init创建对象，指定参数host、port与指定的服务器和端⼝连接，`host默认为localhost`，`port默认为6379`，`db默认为0`**

**语法：**

```python
# 标准写法
sr = StrictRedis(host='localhost', port=6379, db=0)
# 简写
sr = StrictRedis()
```

*根据不同的类型，拥有不同的实例⽅法可以调⽤，与redis命令对应，⽅法需要的参数与命令的参数⼀致*

![类型](https://s3.bmp.ovh/imgs/2021/12/e059ca7725b3d8d3.png)

> 本文基于Redis配置完集成的前提下操作 [`未配置可前往配置哨兵`](https://jiaoyanxia.github.io/2021/12/13/Redis配置哨兵/)

### 示例 ###

- [ ] **前提是电脑或虚拟机的Redis得开启 不然无法访问**

**可以在 StrictRedis 对象中添加 `decode_responses=True`** 

**decode_responses=True属性可以把返回的数据 `自动decode处理 变成字符串`**

```python
# 第一步 导入StrictRedis对象
from redis import StrictRedis

# 配置 StrictRedis 对象
sr = StrictRedis(host='localhost', port=6379, db=0, decode_responses=True)

# 保存一个string类型的键为tname 值为 Shrimps 的数据
result = sr.set('tname', 'Shrimps')

# 查看返回的结果
print(result)  # OK

# 获取tname的值
name = sr.get('tname')
print(f'tname is {name}') # tname is Shrimps

''' 获取我本地redis的第一个数据库的数据 '''
one = sr.key()
print(one)  # 如：tsex, tage, tname
```

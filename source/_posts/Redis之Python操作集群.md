---
title: Redis之Python操作集成
date: 2021-12-13 00:08:00
description: '在Python中使用Redis集成执行操作'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

> 本文基于Redis配置完集成的前提下操作 [`未配置可前往配置集成`](https://jiaoyanxia.github.io/2021/12/13/Redis配置集成/)

### 第一步 安装包 ###

**安装集群配置包**

```python
pip install redis-py-cluster
# 若pip不好使就改为 pip3
```

### 第二步 使用 ###

1. 导包

   ```python
   from rediscluster import *
   ```

   

2. 构建所有的节点

   ```python
   # 构建所有的节点，Redis会使⽤CRC16算法，将键和值写到某个节点上
           startup_nodes = [
               {'host': '192.111.141.129', 'port': '8000'},
               {'host': '192.111.141.129', 'port': '8001'},
               {'host': '192.111.141.129', 'port': '7002'},
               {'host': '192.111.141.129', 'port': '7003'},
               {'host': '192.111.141.129', 'port': '7004'},
               {'host': '192.111.141.129', 'port': '7005'},
           ]
   ```

   

3. 构建RedisCluster对象

   ```python
   # 构建RedisCluster对象
   src = RedisCluster(startup_nodes=startup_nodes, decode_responses=True)
   ```

   

4. 测试设置与获取代码

   ```python
   # 设置键为name、值为itheima的数据
   result = src.set('name', 'aaa')
   print(result)
   # 获取键为name
   name = src.get('name')
   print(name)
   ```

   

5. 再添加异常捕获

```python
if __name__ == '__main__':
    try:
        代码...
      
except Exception as e:
     print(e)

```

## 文件示例 ##

```python
from rediscluster import *


if __name__ == '__main__':
    try:
        # 构建所有的节点，Redis会使⽤CRC16算法，将键和值写到某个节点上
        startup_nodes = [
            {'host': '192.111.141.129', 'port': '8000'},
            {'host': '192.111.141.129', 'port': '8001'},
            {'host': '192.111.141.129', 'port': '7002'},
            {'host': '192.111.141.129', 'port': '7003'},
            {'host': '192.111.141.129', 'port': '7004'},
            {'host': '192.111.141.129', 'port': '7005'},
        ]
        # 构建RedisCluster对象
        src = RedisCluster(startup_nodes=startup_nodes, decode_responses=True)
        # 设置键为name、值为itheima的数据
        result = src.set('name', 'aaa')
        print(result)
        # 获取键为name
        name = src.get('name')
        print(name)
    except Exception as e:
        print(e)
```


---
title: Redis之Python操作哨兵
date: 2021-12-12 00:08:00
description: '在Python中使用Redis哨兵运用主从执行增删改查操作'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

> 本文基于Redis配置完哨兵的前提下操作 [`未配置可前往配置哨兵`](https://jiaoyanxia.github.io/2021/12/12/Redis配置哨兵/)

### 第一步 导包 ###

在虚拟环境中导入`redis`包

```python
pip install redis
# 若pip不好使就改为 pip3
```

### 第二步 使用 ###
1. 导包

   ```python
   from redis import Redis

   from redis.sentinel import Sentinel
   ```

   

2. 配置哨兵地址与端口

   ```python
   # 你的 redis 哨兵的地址和端口
    REDIS_SENTINELS = [
        ('127.0.0.1', '27000'),
        ('127.0.0.1', '27001'),
        ('127.0.0.1', '27002'),
    ]
   ```


3.  获取哨兵对象

    ```python
     # 获取哨兵对象 存到app对象里，在视图中可以通过current_app获取到
    _sentinel = Sentinel(REDIS_SENTINELS)
    ```


4. 监控的节点名字

   ```python
   # 监控的节点名字 mymaster
    REDIS_SENTINEL_SERVICE_NAME = 'mymaster'
   ```


5. 再添加异常捕获

    ```python
    # 通过哨兵对象 获取主节点对象 用来写入信息
    redis_master: Redis = _sentinel.master_for(REDIS_SENTINEL_SERVICE_NAME)
    # 通过哨兵对象 获取从节点对象 用来获取信息
    redis_slave = _sentinel.slave_for(REDIS_SENTINEL_SERVICE_NAME)
    ```

5. 执行数据操作

    ```python
    # 写入数据
    redis_master.hset('person', 'name', 'xiaoming')
    # 获取数据 得到的是byte类型数据 需要decode解码
    name = redis_slave.hget('person', 'name')
    print(name.decode())
    ```
## 文件示例 ##

```python
from redis import Redis

from redis.sentinel import Sentinel

# 你的 redis 哨兵的地址和端口
REDIS_SENTINELS = [
    ('127.0.0.1', '27000'),
    ('127.0.0.1', '27001'),
    ('127.0.0.1', '27002'),
]

# 获取哨兵对象 存到app对象里，在视图中可以通过current_app获取到
_sentinel = Sentinel(REDIS_SENTINELS)

# 监控的节点名字 mymaster
REDIS_SENTINEL_SERVICE_NAME = 'mymaster'


def main():
    # 通过哨兵对象 获取主节点对象 用来写入信息
    redis_master: Redis = _sentinel.master_for(REDIS_SENTINEL_SERVICE_NAME)
    # 通过哨兵对象 获取从节点对象 用来获取信息
    redis_slave = _sentinel.slave_for(REDIS_SENTINEL_SERVICE_NAME)

    # 写入数据
    redis_master.hset('person', 'name', 'xiaoming')
    # 获取数据 得到的是byte类型数据 需要decode解码
    name = redis_slave.hget('person', 'name')
    print(name.decode())


if __name__ == '__main__':
    main()
```


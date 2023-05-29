---
# sticky: 1
title: Redis主从配置
date: 2021-12-10 00:08:00
description: 'Redis数据库在Ubuntu虚拟机中配置主从'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

### 本教程环境基于 Ubuntu 8.3.0 版本 linux 系统配置 Redis 主从

## 主从概念

-   ⼀个 master 可以拥有多个 slave，⼀个 slave ⼜可以拥有多个 slave，如此下去，形成了强⼤的多级服务器集群架构
-   master 用来写数据，slave 用来读数据，经统计：网站的读写比率是 10:1
-   通过主从配置可以实现读写分离

![](https://s3.bmp.ovh/imgs/2021/12/6ab566a17809ae78.png)

## 主从配置

**查看当前主机 IP 地址（记住你的 ip 地址）**

![主机IP地址](https://s3.bmp.ovh/imgs/2021/12/59746ee4e68fb40d.png)

若没有联网可以前往我的[另一篇文章](https://jiaoyanxia.github.io/2021/11/15/win%E7%AB%AF%E6%93%8D%E6%8E%A7VM%E8%99%9A%E6%8B%9F%E6%9C%BA/)配置网络

#### 删除 redis 默认执行的 ip 端口

> 若不删除的话 默认会执行 彻底清除完毕再开启主从配置

查看当前 redis 所有进程

```python
ps -aux|grep redis
```

除了后缀为 `grep --color=auto redis` 其余全都删除

```python
# kill 删除命令 -9 强制 23131端口号
# 端口为你要删除的进程的第二个字段(名称后面的数字)
sudo kill -9 23131
```

### 配置主

1. 进入 /etc/redis 文件目录修改 `redis.conf` (主)文件

    ```python
    # 进入目录
    cd /etc/redis
    # 用记事本修改文件
    sudo gedit redis.conf
    ```

    _修改内容如下：_

    **开启第二行的 bind 修改为你本机的 ip 地址 端口不用改**

    ![](https://s3.bmp.ovh/imgs/2021/12/5c5eed97e96d9357.png)

2. 重启 redis 服务

    ```python
    # 重启主服务器
    sudo service redis stop

    # 查看对应ip及端口号的主进程是否开启
    # 主端口号默认为(6379)
    ps -aux|grep redis

    # 运行主服务器
    sudo redis-server /etc/redis/redis.conf
    ```

    _如果运行成功并且可以运行 redis 环境与命令 `主`就配置成功了_

### 配置从

1. 进入 /etc/redis 文件目录复制 `redis.conf` (主)文件

    ```python
    # 进入目录
    cd /etc/redis
    # 复制主文件
    sudo cp redis.conf ./redis1.conf
    ```

    现在的从文件为`redis1.conf`（从）

2. 修改从服务器配置文件

    ```python
    # 用记事本修改文件
    sudo gedit redis1.conf
    ```

    _修改内容如下：_

    **第一步是一样的 因为需绑定一样的端口**

    ![](https://s3.bmp.ovh/imgs/2021/12/5c5eed97e96d9357.png)

    **第二步 `ctrl+f` 找到 port 端口配置 更改为`port 6380`**

    ![](https://s3.bmp.ovh/imgs/2021/12/601f0f5931152768.png)

    **第三步 同样找到 slaveof 新建一行 添加从属于哪个主服务器**

    ![](https://s3.bmp.ovh/imgs/2021/12/b46f6318555c48e9.png)

    _如果运行成功并且可以运行 redis 环境与命令 `主`就配置成功了_

### 测试主从

**一图解：**

这里我开启三个终端分别用来测试进程，主 ，从

1. 第一步`ps -aux|grep redis` 查看所有进程

2. 第二步运行主从

    **主：** `sudo redis-server /etc/redis/redis.conf`

    **从：** `sudo redis-server /etc/redis/1redis.conf`

3. 再次查看进程后，开始使用

![查看进程](https://s3.bmp.ovh/imgs/2021/12/04cc9b187825991c.png)

4. 通过主创建从获取测试成功
   ![主从](https://s3.bmp.ovh/imgs/2021/12/0ef5a16c0f523db7.png)

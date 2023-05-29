---
title: Redis配置哨兵
date: 2021-12-12 00:08:00
description: 'Ubuntu虚拟机端配置Redis哨兵'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

### Redis sentinel（哨兵） ###

**主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。**这不是一种推荐的方式，更多时候，我们优先考虑**哨兵模式**。

### 哨兵概论与运行机制 ###

哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**

![哨兵运行机制](https://upload-images.jianshu.io/upload_images/11320039-57a77ca2757d0924.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

### Redis Sentinel的主要功能 ###

Sentinel的主要功能包括主节点存活检测、主从运行情况检测、自动故障转移（failover）、主从切换。Redis的Sentinel最小配置是一主一从。 Redis的Sentinel系统可以用来管理多个Redis服务器，该系统可以执行以下四个任务：

- **`监控`**

  Sentinel会不断的检查主服务器和从服务器是否正常运行。

- 通知

  当被监控的某个Redis服务器出现问题，Sentinel通过API脚本向管理员或者其他的应用程序发送通知。

- **`自动故障转移`**

  当主节点不能正常工作时，Sentinel会开始一次自动的故障转移操作，它会将与失效主节点是主从关系的其中一个从节点升级为新的主节点， 并且将其他的从节点指向新的主节点。

  > 当哨兵监测到master宕机，会自动将slave切换成master，然后通过**发布订阅模式**通知其他的从服务器，修改配置文件，让它们切换主机

- 配置提供者

  在Redis Sentinel模式下，客户端应用在初始化时连接的是Sentinel节点集合，从中获取主节点的信息。

### Redis配置哨兵模式 ###

如图：配置3个哨兵和1主2从的Redis服务器来演示这个过程。

![Redis配置哨兵模式](https://upload-images.jianshu.io/upload_images/11320039-3f40b17c0412116c.png?imageMogr2/auto-orient/strip|imageView2/2/w/747/format/webp)

#### 第一步 配置一主两从 ####

**如果不会配主从的话可以去查看我之前的文章 [`主从配置`](https://jiaoyanxia.github.io/2021/12/10/Redis%E4%B8%BB%E4%BB%8E%E9%85%8D%E7%BD%AE/)**

1. 在原先的从基础上再拷贝一个从

   ```python
   # cp 要拷贝的文件 拷贝完的文件
   sudo cp redis1.conf redis2.conf
   ```

2. 更改从的配置端口

   ```python
   # 进入文本编辑
   sudo gedit /etc/redis/redis2.conf
   # 找到 port 更改为 6381 # 有一个从的端口为 6380
   port 6381
   ```

3. 测试运行

   ```python
   ps -aux|grep redis
   ```

   ##### *如果出现 默认的主从进程 **就强制删除 强制删除不了强制停止*** #####

   ```python
   # 强制停止
   /etc/init.d/redis-server stop
   # 执行完要求你输入密码
   ```

   

   ![强制停止](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131014030.png)

#### 第二步 配置三哨兵 ####

配置3个哨兵，每个哨兵的配置都是一样的。在Redis安装目录下有一个sentinel.conf文件，copy一份进行修改。如果没有 就自行创建一个文件 (`sudo touch xxx`)在里面更改配置

**配置哨兵文件如下：**

##### 三个哨兵 分别在27000 2700127002 三个端口上

**重点更改下面 `第二行的的端口号` 以及 `第十七行的主服务器ip端口地址`**

```python
# 端口
port 27000

# 是否后台启动
daemonize yes

# pid文件路径
pidfile /var/run/redis-sentinel.pid

# 日志文件路径
logfile "/var/log/redis/sentinel.log"

# rdb数据备份的目录
dir /var/lib/redis

# 定义Redis主的别名, IP, 端口，这里的2指的是需要至少2个Sentinel认为主Redis挂了才最终会采取下一步行为
sentinel monitor mymaster 192.xxx.xx.128 6379 2

# 如果mymaster 30秒内没有响应，则认为其主观失效
sentinel down-after-milliseconds mymaster 30000

# 如果master重新选出来后，其它slave节点能同时并行从新master同步数据的台数有多少个，显然该值越大，所有slave节点完成同步切换的整体速度越快，但如果此时正好有人在访问这些slave，可能造成读取失败，影响面会更广。最保守的设置为1，同一时间，只能有一台干这件事，这样其它slave还能继续服务，但是所有slave全部完成缓存更新同步的进程将变慢。
sentinel parallel-syncs mymaster 1

# 该参数指定一个时间段，在该时间段内没有实现故障转移成功，则会再一次发起故障转移的操作，单位毫秒
sentinel failover-timeout mymaster 180000

# 不允许使用SENTINEL SET设置notification-script和client-reconfig-script
sentinel deny-scripts-reconfig yes
```

##### 哨兵执行命令 #####

```python
# 开启哨兵 文件名后面添加--sentinel
sudo redis-server sentine01.conf --sentinel
```

#### 第三步 建立连接  ####

用redic-cli  连接服务器后 检查主从状态

```python
# 进入服务器后 执行命令
info  replication
```

##### 主状态： #####

![主状态](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131019794.png)

##### 从状态： #####

![从状态](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131023602.png)

##### 启动主从服务器与哨兵 #####

这里就不演示代码了 -  **最后执行状态**

![OwO](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131044085.png)

可以自己尝试全部开始后去删除主服务器从服务器的**`自动故障转移`**一般都是转移到默认从服务

当**主服务器(旧)**再启动时，已经转移为当前主服务器的从服务器了。

参考文章：[秃头哥编程之Redis哨兵](https://www.jianshu.com/p/06ab9daf921d)
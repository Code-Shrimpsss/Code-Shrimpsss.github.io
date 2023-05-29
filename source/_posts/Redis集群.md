---
title: Redis配置集成
date: 2021-12-13 00:08:00
description: 'Ubuntu虚拟机端配置Redis集成'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

### Redis集群介绍 ###

集群是一组相互独立的、通过高速网络互联的计算机，它们构成了一个组，并以单一系统的模式加以管理。**一个客户与集群相互作用时，集群像是一个独立的服务器。**

Redis集群是一个由**多个主从节点群**组成的分布式服务集群，它具有**复制、高可用和分片**特性。

Redis集群不需要sentinel哨兵也能完成节点移除和故障转移的功能。需要将每个节点设置成集群模式，这种集群模式没有中心节点，可水平扩展，据官方文档称可以线性扩展到上万个节点(官方推荐不超过1000个节点)。redis集群的性能和高可用性均优于之前版本的哨兵模式，且集群配置非常简单。

![集群执行机制](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131711498.png)

### Redis集群的优点 ###

1. Redis集群有多个master，可以**减小访问瞬断问题的影响**；
2. 若集群中有一个master挂了，正好需要向这个master写数据，这个操作需要等待一下；但是向其他master节点写数据是不受影响的。
3. Redis集群有多个master，可以提供更高的并发量；　　
4. Redis集群可以分片存储，这样就可以存储更多的数据；

### 集群的配置与使用 ###

**前提：** 准备`两台虚拟机各开3个redis` 或者 `一台虚拟机开6个redis `

**其中三个为主服务器 每个主服务器配备一个从服务器**

##### 这里用一台虚拟机开6个redis示例 #####

#### 第一步 清除 ####

1. 进入你的redis目录

   ```pyth
   cd /etc/redis
   ```

   

2. 查看目录文件 把多余的文件清除(如果只是redis服务器与哨兵 则不用清除)

   ```python
   # 查看文件
   ls
   # 清除文件命令
   sudo rm '文件'
   ```

   

#### 第二步 配置集群 ####

1. 在/etc/redis目录下创建6个redis 这里分别为 **7000.conf 递增**

   ```python
   # 创建文件
   sudo touch 7000.conf
   sudo touch 7001.conf
   sudo touch 7002.conf
   sudo touch 7003.conf
   sudo touch 7004.conf
   sudo touch 7005.conf
   ```

2. 编辑文件内容

   **第一步：用记事本打开**

   ```python
   sudo gedit 7000.conf
   ```

   **第二步：添加文件内容**

   ```python
   port 7000
   bind 172.16.179.130
   daemonize yes
   pidfile 7000.pid
   cluster-enabled yes
   cluster-config-file 7000_node.conf
   cluster-node-timeout 15000
   appendonly yes
   ```

   - port 为当前这个的redis的端口 
   - bind 为你的ip地址
   - pidfile 同为改为当前这个的redis端口 .pid
   - cluster-config-file  同为改为当前这个的redis端口 _node.conf

   **后续的其他redis文件重点就配置这几样 `端口记得更改` 最简单的方法就是递增(如7001 7002)**

   **总结：**三个⽂件的配置区别在port、pidfile、cluster-config-file三项

3. 使⽤配置⽂件启动redis服务

   ```python
   redis-server 7000.conf
   redis-server 7001.conf
   redis-server 7002.conf
   redis-server 7003.conf
   redis-server 7004.conf
   redis-server 7005.conf
   ```

4. 查看当前进程状态

   ```python
   ps -aux|grep redis
   ```

   ![进程状态](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131622036.png)

   > 如果缺少或者没被添加进程 则`更改文件内容的端口号`
   >
   > 这里我更改了几个端口号 原因是同局域网太多请求导致挂不上 更改端口号即可

5. 5.0以后开启集群的命令

   **--cluster-replicas 1  这里的1 表示一个主有1个从**

   ```python
   # 命令语法： redis-cli --cluster create ip地址: 端口 ip地址:端口 等...
   # 示例
   redis-cli --cluster create 192.168.49.128:8000 192.168.49.128:8001 192.168.49.128:7002 192.168.49.140:7003 192.168.49.140:7004 192.168.49.140:7005 --cluster-replicas 1
   ```

   执行完后如果出现以下信息，就表示启动成功了！

   ![开启集群](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131600604.png)

6. 集群使用

   **命令语法：**`redis-cli -h xxxx -p xxx -c`

   ![集群使用](https://cdn.jsdelivr.net/gh/jiaoyanxia/picgo@master/blogimg/202112131632064.png)



> 参考文章：[风止雨歇 - Redis集群](https://www.cnblogs.com/yufeng218/p/13688582.html)
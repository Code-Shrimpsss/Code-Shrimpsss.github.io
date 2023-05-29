---
title: Redis安装
date: 2021-12-09 00:08:00
description: 'Redis的三种安装方式'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

#### 官网 ####

##### 英文：<https://redis.io/> #####

##### 中文：<http://www.redis.cn/> #####

参考：`[菜鸟Redis](https://www.runoob.com/redis/redis-install.html)`

### 安装方式一  ###

- 使用apt，**注意提前检查下apt的源**，如果不对 就修改[镜像源](https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.3e221b11jRXX1I)
- 安装前 `sudo apt-get update`
- 安装`redis`命令如下：

```python
sudo apt-get install redis-server 
```

**注意：安装完后关闭终端重新开启，才能使用**

- 安装完后检查 执行程序都在 usr/bin/下

  ![查看文件](https://s3.bmp.ovh/imgs/2021/12/9af8447f2193709e.png)

  

- 默认安装后会启动，命令如下

  ```python
  sudo service redis-server start # 启动
  sudo service redis-server stop  # 停止
  sudo service redis-server restart  # 重启
  ```

- 查看进程 

  ```python
  ps -aux|grep redis
  ```

  如果出现 **redis-server *:6379 `默认端口 6379`** ，说明redis正在运行

  ![redis 查看进程](https://s3.bmp.ovh/imgs/2021/12/7f969644330b379d.png)

- 进入操作环境

  现在我们输入 `redis-cli` 命令进入操作环境。

  127.0.0.1 是本机 IP ，6379 是 redis 服务端口。

  再执行 `ping` 命令查看状态。

  ![redis - ping](https://s3.bmp.ovh/imgs/2021/12/3322b60f84807863.png)

  如果出现 `PONG`  ，说明已经成功安装了redis!

  

- **对应的这种安装方式中出错的卸载方案**：

  ```python
  sudo apt-get purge --auto-remove redis-server
  --auto-remove  删除 redis-server # 软件包和不再需要的其他相关软件包
  purge  # 删除 redis-server的本地/配置文件
  ```

### 安装方式二  【源码安装】 ###

**下载地址：**<http://redis.io/download>，下载最新稳定版本。

```python
# wget http://download.redis.io/releases/redis-6.0.8.tar.gz
# tar xzf redis-6.0.8.tar.gz
# cd redis-6.0.8
# make
```

执行完 **make** 命令后，redis-6.0.8 的 **src** 目录下会出现编译后的 redis 服务程序 redis-server，还有用于测试的客户端程序 redis-cli：

下面启动 redis 服务：

```python
# cd src
# ./redis-server
```

注意这种方式启动 redis 使用的是默认配置。也可以通过启动参数告诉 redis 使用指定配置文件使用下面命令启动。

```python
# cd src
# ./redis-server ../redis.conf
```

**redis.conf** 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。

启动 redis 服务进程后，就可以使用测试客户端程序 redis-cli 和 redis 服务交互了。 比如：

```python
# cd src
# ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

## Ubuntu 安装下载 ##

在 Ubuntu 系统安装 Redis 可以使用以下命令:

```python
sudo apt update  # 更新 apt 
sudo apt install redis-server  # 安装redis
```

**如果以上出报错现镜像源，则改动镜像源操作：**

前提：[配置及更换清华镜像源](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)

> 编程环境为`Ubuntu`终端  (**>>>** 为执行代码)

1. 查看源目录

   ```python
   # 进入源文件目录 
   >>> cd /etc/apt/  
   # 查看目录结构
   >>> ls 
   # 找到 sources.list 文件
   sources.list
   ```

2. 先把 sources.list 文件中的 `https` 后面的 `s` 去掉成(http)

   ```python
   # 编辑镜像源文件
   >>> sudo gedit sources.list
   
   '''弹出txt后手动更改'''
   ```

3. 更改后 重新更新源文件

   ```python
   sudo apt-get update
   ```

4. 再执行命令

   ```python
   sudo apt-get install -y ca-certificates
   ```

5. 执行完以上两个命令后，回到 sources.list 文件中的把 `http` 后面加 `s` (`改回https`)

   ```python
   # 编辑镜像源文件
   >>> sudo gedit sources.list
   ```

6. 更改后 重新更新源文件

   ```python
   sudo apt-get update
   ```

7. 重新安装 `redis`

   ```python
   sudo apt-get install redis-server
   ```

8. 查看进程 

   ```python
   ps -aux|grep redis
   ```

   如果出现 **redis-server *:6379 `默认端口 6379`** ，说明redis正在运行

   ![redis 查看进程](https://s3.bmp.ovh/imgs/2021/12/7f969644330b379d.png)

9. 进入操作环境

   现在我们输入 `redis-cli` 命令进入操作环境。

   127.0.0.1 是本机 IP ，6379 是 redis 服务端口。

   再执行 `ping` 命令查看状态。

   ![redis - ping](https://s3.bmp.ovh/imgs/2021/12/3322b60f84807863.png)

   如果出现 `PONG`  ，说明已经成功安装了redis!

   

### 启动 Redis ###

```
# redis-server
```

### 查看 redis 是否启动？ ###

```
# redis-cli
```

以上命令将打开以下终端：

```
redis 127.0.0.1:6379>
```

127.0.0.1 是本机 IP ，6379 是 redis 服务端口。现在我们输入 PING 命令。

```
redis 127.0.0.1:6379> ping
PONG
```

以上说明我们已经成功安装了redis。

### 其他问题 ###

##### [无法在 ubuntu 18.04 上安装 redis-server](https://stackoverflow.com/questions/50668845/cannot-install-redis-server-on-ubuntu-18-04) #####


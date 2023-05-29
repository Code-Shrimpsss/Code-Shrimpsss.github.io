---
title: Redis基本操作
date: 2021-12-09 00:08:00
description: 'Redis之客户端与服务器端的基本操作'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---
![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

### 基本命令 ###

#### 服务器端 ####

- 启动服务端

```python
redis-server
```

- 查看帮助手册

```python
redis-server --help
```

- 查看redis服务器进程

```python
ps -aux|grep redis
```

- 杀死redis服务器

```python
# sudo kell 删除 -9 强制 指定服务器 (强制删除命令)
sudo kell -9 pid
```

- 使用指定的配置文件开启redis服务

```python
# sudo redis-server 源路径
sudo redis-server /etc/redis/redis.conf
```

#### 客户端 ####

- 启动客户端

```python
redis-cli
```

- 查看帮助手册

```python
redis-cli --help
```

- 运行测试命令

```python
# 客户端中
ping
```

- 切换数据库

```python
# select [0-15] 默认使用第一个，共有16个数据库
select 7
```


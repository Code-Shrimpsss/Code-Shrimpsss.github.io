---
title: Docker之容器
date: 2022-1-06 00:08:00
tag: Docker
description: Docker的容器语法与解析
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fcdn.ancii.com%2Farticle%2Fimage%2Fv1%2FQj%2Fzb%2FRO%2FORzjbQVFJasX5aR2DaTg2_Zx5KSH75KdTtVIpx5BAiZSOFE6UDSXmdPNK0vBK1kx6D9KnKYbT3PZJrhT421k4RvmNO9bq5YW3aUlHXtRyug.jpg&refer=http%3A%2F%2Fcdn.ancii.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644045441&t=0e3bb5c7d14b370a1b1078f47965ecce
---


![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2Fimg_convert%2F581f1fdc52024d3994a5699eee607009.png&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644045266&t=53bf23ad024e5a2049a7877522d9c124)

### 容器介绍 ###

镜像（`Image`）和容器（`Container`）的关系，就像是面向对象程序设计中的`类`和`实例`一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

> 如果提示权限不足，在每行命令前添加 `sudo`  ；本文中大部分命令已经自动添加。

若不想输入 `sudo` 可配置以下内容：

1. `sudo groupadd docker`     #添加名字为docker用户组  默认可能有了docker组了
2. `sudo gpasswd -a $USER docker`     #将登陆用户加入到docker用户组中
3. `newgrp docker`     #更新用户组
4. `docker ps`    #测试docker命令是否可以使用sudo正常使用

### 查看容器 ###

显示正在运行的容器

```python
docker ps
```

显示所有运行过的容器，包括已经不运行的容器

```python
docker ps -a
```

> **注意：管理docker容器可以通过名称，也可以通过ID**



### 启动容器 ###

- 守护进程方式启动容器


```python
docker run [参数] docker_image [执行的命令]
```

- `-d` 让Docker容器在后台以守护形式运行。

```python
docker ps -d redis
```

- `-t `  参数让Docker分配一个伪终端并绑定到容器的标准输入上

- `-i`  则让容器的标准输入保持打开

```python
docker run -i -t redis /bin/bash
```

- `--name` 选项来给容器设置一个名字并打开运行

```python
docker run --name redis -dit redis:v1
```

##### 常用搭配命令 #####

- 启动并直接进入容器中

```python
docker run -it redis:v1 /bin/bash  # redis:v1 为容器
```

- 启动起别名并进入容器中

```python
docker run --name 容器别名 -d redis:v1 /bin/bash   # redis:v1 为容器
```



### 操作容器 ###

###### 启动已终止的容器 ######

在生产过程中，常常会出现运行和不运行的容器，我们使用 start 命令开起一个已关闭的容器

```python
docker start [container_id]
```

###### 关闭容器 ######

在生产中，我们会以为临时情况，要关闭某些容器，我们使用 stop 命令来关闭某个容器

```python
docker stop [container_id]
```

###### 重启停止的容器 ######

停止的容器可以通过 docker restart 重启：

```python
docker restart [container_id]
```



### 删除容器 ###

##### 正常删除容器 -- `删除已经关闭的` #####

```python
docker rm [container_id]
```

##### 强制删除运行容器 -- `删除正在运行的` #####

```python
docker rm -f [container_id]
```

##### 拓展批量关闭容器 #####

```python
docker rm -f $(docker ps -a -q)
```



### 进入正在运行的容器 ###

```python
docker exec [选项] 容器id/容器名 命令
```

只有 `-i` 参数时，由于没有分配伪终端，没有命令提示符

```python
docker exec -i redis11 /bin/bash
```

当 `-i-t` 参数一起使用时，则可以使用 Linux 命令提示符

```python
docker exec -it redis11 /bin/bash
```



### 查看容器详细信息 ###

```python
docker inspect [容器id]
```



### 查看容器日志 ###

```python
docker logs [容器id]
```

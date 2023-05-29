---
title: Docker之镜像
date: 2022-1-06 00:08:00
tag: Docker
description: Docker之镜像语法与解析
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fcdn.ancii.com%2Farticle%2Fimage%2Fv1%2FQj%2Fzb%2FRO%2FORzjbQVFJasX5aR2DaTg2_Zx5KSH75KdTtVIpx5BAiZSOFE6UDSXmdPNK0vBK1kx6D9KnKYbT3PZJrhT421k4RvmNO9bq5YW3aUlHXtRyug.jpg&refer=http%3A%2F%2Fcdn.ancii.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644045441&t=0e3bb5c7d14b370a1b1078f47965ecce
---


![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2Fimg_convert%2F581f1fdc52024d3994a5699eee607009.png&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644045266&t=53bf23ad024e5a2049a7877522d9c124)

### 镜像介绍 ###

Docker 镜像是一个**特殊的文件系统**，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。

> 如果提示权限不足，在每行命令前添加 `sudo`  ；本文中大部分命令已经自动添加。

若不想输入 `sudo` 可配置以下内容：

1. `sudo groupadd docker`     #添加名字为docker用户组  默认可能有了docker组了
2. `sudo gpasswd -a $USER docker`     #将登陆用户加入到docker用户组中
3. `newgrp docker`     #更新用户组
4. `docker ps`    #测试docker命令是否可以使用sudo正常使用

### 查看镜像 ###

命令格式：

```python
sudo docker images <image_name>
```

命令演示：

```python
sudo docker images ubuntu
```

列出本地所有镜像

```python
sudo docker images 
# or
sudo docker images -a
```



### 搜索镜像 ###

命令格式：

```python
docker search [image_name]
```

命令演示：

```python
docker search redis
```



### 获取镜像 ###

可自行前往官网获取想要的镜像源： [点我](https://hub.docker.com/search/?q=&type=image)

命令格式：

```python
sudo docker pull [image_name]
```

命令演示： 

```python
# 这里用redis演示
sudo docker pull redis
```

###### 获取过程如下  ######

![获取成功](https://s2.loli.net/2022/01/06/6Ia39vgoAiK2zHN.png)

> **如果中途加载失败可能是网络问题 多试试几次或者更换镜像源**



### 重命名镜像 ###

命令格式：

```python
docker tag [old_image]:[old_version] [new_image]:[new_version]
```

命令演示：

```python
docker tag redis:latest redis:v1
```

###### 重命名过程如下 ######

![重命名镜像](https://s2.loli.net/2022/01/06/AXPu5HjTnZkdh6m.png)

### 删除镜像 ###

命令格式：

```python
docker rmi [image_id/image_name:image_version]
```

命令演示：

```python
# 这里演示删除 redis镜像 版本为 v2
docker rmi redis:v2
```

###### 删除过程如下 ######

![删除镜像](https://raw.githubusercontent.com/jiaoyanxia/picgo/master/img/20220106144519.png)



### 导出镜像 ###

**将已经下载好的镜像，导出到本地，以备后用**

导出镜像命令格式：

```python
docker save -o [包文件] [镜像]
# or
docker save [镜像1] ... [镜像n] > [包文件]
```

命令演示：

```python
cdocker save -o redis.tar redis
```

> **注意： docker save 会保存镜像的所有历史记录和元数据信息**

###### 导出过程如下 ######

![导出](https://raw.githubusercontent.com/jiaoyanxia/picgo/master/img/20220106150443.png)

### 导入镜像 ###

导入镜像命令格式：

```python
docker load < [image.tar_name]
# or
docker load -i [image.tar_name]
```

演示指令：

```python
# 这里用打包完的镜像进行导入
docker load -i redisv1.tar
```

###### 导入过程如下 ######

![导入](https://raw.githubusercontent.com/jiaoyanxia/picgo/master/img/20220106150713.png)

### 查看镜像历史 ###

```python
docker history [image_name]
```


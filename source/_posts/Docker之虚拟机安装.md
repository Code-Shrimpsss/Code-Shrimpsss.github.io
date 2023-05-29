---
title: Docker之简介与配置
date: 2022-1-06 00:08:00
tag: Docker
description: Docker介绍与虚拟机安装配置
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fcdn.ancii.com%2Farticle%2Fimage%2Fv1%2FQj%2Fzb%2FRO%2FORzjbQVFJasX5aR2DaTg2_Zx5KSH75KdTtVIpx5BAiZSOFE6UDSXmdPNK0vBK1kx6D9KnKYbT3PZJrhT421k4RvmNO9bq5YW3aUlHXtRyug.jpg&refer=http%3A%2F%2Fcdn.ancii.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644045441&t=0e3bb5c7d14b370a1b1078f47965ecce
---

![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2Fimg_convert%2F581f1fdc52024d3994a5699eee607009.png&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644045266&t=53bf23ad024e5a2049a7877522d9c124)

### Docker简介 ###

Docker 是一个开源的`应用容器引擎`，可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口,更重要的是容器性能开销极低。

Docker三大基本概念:： `镜像（Image） `   `容器（Container）`   `仓库（Repository）`

Docker官网：http://www.docker.com

Github Docker源码：https://github.com/docker/docker

### 虚拟机安装Docker ###

#### 安装Docker ####

1. 下载文件到虚拟机中

   传输链接：https://cowtransfer.com/s/3248e7e335ed4b 或 打开【奶牛快传】cowtransfer.com 使用传输口令：xhzomm 提取；

2. 配置命令

   ```python
   # 源码目录（docker_sc）
   cd docker_sc
   
   # 配置命令
   sudo apt-key add gpg
   sudo dpkg -i containerd.io_1.3.9-1_amd64.deb
   sudo dpkg -i docker-ce-cli_19.03.15_3-0_ubuntu-bionic_amd64.deb
   sudo dpkg -i docker-ce_19.03.15_3-0_ubuntu-bionic_amd64.deb
   ```

   

3. 测试

   ```python
   # -v 与 info 都可以运行 
   sudo docker -v
   sudo docker info
   ```

   

   

#### 虚拟机中配置镜像 ####

1. 配置国内镜像加速器(如果已配置跳过第二条)

   - [Docker 官方提供的中国 registry mirror`https://registry.docker-cn.com`](https://docs.docker.com/registry/recipes/mirror/#use-case-the-china-registry-mirror)
   - [阿里云加速器(需登录账号获取)](https://cr.console.aliyun.com/cn-hangzhou/mirrors)
   - [七牛云加速器`https://reg-mirror.qiniu.com/`](https://kirk-enterprise.github.io/hub-docs/#/user-guide/mirror)

   > 当配置某一个加速器地址之后，若发现拉取不到镜像，请切换到另一个加速器地址

   

2. 在`/etc/docker/daemon.json`中写入如下内容（如果文件不存在请新建该文件）

   ```python
   {
     "registry-mirrors": [
       "https://registry.docker-cn.com"
     ]
   }
   ```

   

3. 重启服务

   ```python
   $ sudo systemctl daemon-reload
   $ sudo systemctl restart docker
   ```

   


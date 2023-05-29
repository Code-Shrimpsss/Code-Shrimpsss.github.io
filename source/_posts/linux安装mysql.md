---
title: 'linux系统安装Mysql'
date: 2021-9-25 00:00:00
descover: 'linux系统安装Mysql'
tag: Mysql
cover: https://s3.bmp.ovh/imgs/2021/12/b0d01eb062a97de0.pngcription
---

# linux安装mysql #

------

查阅文献：https://blog.csdn.net/qq_42468130/article/details/88595418

**前提 ： 如果有mysql环境 先行卸载之前的版本**

### 卸载 ###

1. 查看mysql的依赖项

   ```python
   dpkg --list|grep mysql
   ```

   

2. 卸载

   ```python
   sudo apt-get remove mysql-common
   ```

   

3. 卸载( 最后的版本数字根据自己具体的版本进行相应的修改 )

   ```python
   sudo apt-get autoremove --purge mysql-server-5.7
   ```

   

4. 清除残留数据

   ```python
   dpkg -l|grep ^rc|awk '{print$2}'|sudo xargs dpkg -P
   ```

   

5. 再次查看依赖项 `如果打印为空 说明依赖完全删除`

   ```python
   dpkg --list|grep mysql
   ```

   

   **若仍有其他残留内容，则继续清除剩余依赖项**

   ```python
   sudo apt-get autoremove --purge mysql-apt-config
   ```

   

6. 再次查看依赖项与目录文件

   ```python
   # 1.查看依赖项为空
   dpkg --list|grep mysql
   # 2.进入etc/mysql 目录查看是否有mysql这个文件内容
   cd /
   cd /etc/mysql 
   ```

   **如果为空或没有这个文件 则已经完全删除**



### 安装  ###

1. 使用命令下载软件包

   ```python
   sudo apt-get install mysql-server
   ```

   

2. 查找默认用户名密码

   ```python
   cd /
   cd /etc/mysql
   sudo cat debian.cnf
   # 其中的 user = xxxx 与 password = xxx 分别为你的账号与密码
   ```

   

3. 登录

   ```python
   # 语法： mysql -uxxx -pxxx 
   # xxx为上述查看到的用户与密码 如:
   mysql -umyname -p123456
   
   ```

   **登录成功后输入环境为 ：** `mysql>`

   

4. 修改密码

   ```python
   # 1. 使用mysql
   use mysql;
   # 2. 修改密码（密码为括号引号包裹的区域 在里面更改你的密码）账号为最后的字符串'root'
   update user set authentication_string = PASSWORD (' 密码 ') where User='root';
   # 3. 更新密码
   update user set plugin='mysql_native_password';
   # 4. 更新权限
   flush privileges;
   # 5. 退出mysql
   quit;
   ```

   

5. 重启mysql

   ```python
   sudo service mysql restart;
   ```

   

6. 重新登录

   ```python
   mysql -u你的账号 -p你的密码
   ```

   **进入 `mysql>` 环境后， 就可以开始你的mysql世界了~~**

### 问题解析 ###

##### apt源出问题: #####

1. 进入 /etc/apt目录

   ```python
   cd /
   cd /etc/apt
   ```

   

2. 进入sources.list.d目录

   ```python
   cd sources.list.d
   ```

   

3. 把里面目录里的内容清空

   ```python
   sudo rm *
   ```

   

4. 返回到apt目录

   ```python
   cd..
   ```

   

5. 打开sources.list

   ```python
   sudo gedit sources.list
   
   #输入密码
   #OP
   ```

   

6. 把里面的文件替换

   ```python
   # 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释 deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse # 预发布软件源，不建议启用 # deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
   ```

   保存关闭。 

   然后关闭终端 

   重新打开终端 执行sudo apt-get update  就成功了

**如果遇到Certifificate verifification failed: The certifificate is NOT trusted.错误 **

```python
1. 把sources.list里的所有请求地址 改为http 然后再sudo apt-get update
2. 安装ca-certificates
3. 改为https 再sudo apt-get update
```


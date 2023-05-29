---
title: Django搭建
date: 2021-11-22 00:08:00
tag: Django
description: Django在Vm虚拟机上搭建
cover: https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto
---

![](https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto)

# Django简介与安装 #

------

Django 是一个开放源代码的 Web 应用框架，由 Python 写成，[点此查看Django官方文档](https://docs.djangoproject.com/en/3.2/)

> 官网说法： *Django makes it easier to build better web apps more quickly and with less code.*

适合有Python基础的人学习  [跳转点击学习python](https://docs.djangoproject.com/en/3.2/)

如果你已经具备基本的Python开发使用,接下来可以开始安装你的Django了

**本文使用搭配Win10配置**

### MVT模型 ###

Django 采用了 **MVT** 的软件设计模式，即模型（Model），视图（View）和模板（Template）

- M 表示模型（Model）：编写程序应有的功能，负责业务对象与数据库的映射(ORM)。
- T 表示模板 (Template)：负责如何把页面(html)展示给用户。
- V 表示视图（View）：负责业务逻辑，并在适当时候调用 Model和 Template。

**通过URL分发器将一个个URL的页面请求分发给不同的View处理,View再调用相应的Model 和Templte**

MTV 的响应模式如下所示：

![MVT](https://www.runoob.com/wp-content/uploads/2020/05/MTV-Diagram.png)

### 安装 ###

**前提是有Python的环境**，如没安装,可前往官网安装

#### 搭建虚拟环境 ####

`虚拟环境`可以搭建独立的`python运行环境`, 使得单个项目的运行环境与其它项目互不影响.

```python
 # win端
 pip install django==2.2.5
 # 如果是VM虚拟机或linux则在前面加sudo
 sudo pip install django==2.2.5
```

##### 安装虚拟环境 #####

```python
# virtualenv 虚拟环境的包
pip install virtualenv   
    
# virtualenv 虚拟环境辅助工具包
pip install virtualenvwrapper
    
# 虚拟环境辅助工具包有对应的windows版本
pip install virtualenvwrapper-win
```

##### 使用虚拟环境 #####

```python
''' 如 创建一个虚拟环境 叫django_new'''

mkvirtualenv 虚拟环境名称
-- 例 ：
mkvirtualenv django_new

# 展示所有的虚拟环境   
workon  

# 进入虚拟环境
workon  虚拟环境的名字  
-- 例 ：
workon  django_new
    
# 退出虚拟环境
deactivate

# 删除虚拟环境
rmvirtualenv 虚拟环境名称

1.先退出：deactivate
2.再删除：rmvirtualenv django_new
```

##### 在虚拟环境中安装工具包 #####

```python
pip install 包名称

# 例 : 安装django-2.2.5的包
pip install django==2.2.5

# 查看虚拟环境中安装的包
pip list
```

#### 创建Django项目 ####

例如创建一个名为 bookmanager 的项目

```python
django-admin starproject bookmanager
```

**切记：**Pycharm中的运行解释器切换到虚拟环境中的解释器

```python
# 运行开发服务器
# 可以不写IP和端口，默认IP是127.0.0.1，默认端口为8000。
python manage.py runserver
-- 或：
python manage.py runserver ip:端口
```

**django默认工作在调式Debug模式下，如果增加、修改、删除文件，服务器会自动重启。**

***按ctrl+c停止服务器***

#### 创建子应用 ####

在django中，创建子应用模块目录仍然可以通过命令来操作，例:

```django
python manage.py startapp 子应用名称
```

**manage.py**为上述创建工程时自动生成的管理文件

##### 子应用目录说明 #####

- **admin.py**文件跟网站的后台管理站点配置相关。
- **apps.py**文件用于配置当前子应用的相关信息。
- **migrations**目录用于存放数据库迁移历史文件。
- **models.py**文件用户保存数据库模型类。
- **tests.py**文件用于开发测试用例，编写单元测试。
- **views.py**文件用于编写Web应用视图。

###### 注册子应用 ######

> 在apps.py中的Config类添加到**INSTALLED_APPS列表中**。
>
> 如：INSTALLED_APPS = [ ....... , '我添加的一个应用']
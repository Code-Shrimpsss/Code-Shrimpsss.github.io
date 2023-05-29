---
title: Django数据库开发
date: 2021-11-30 00:08:00
tag: Django
description: Django数据库配置与使用
cover: https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto
---

![](https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto)

**使用Django进行数据库开发的提示 ：**

- `MVT`设计模式中的`Model`, 专门负责和数据库交互.对应`(models.py)`
- 由于`Model`中内嵌了`ORM框架`, 所以不需要直接面向数据库编程.
- 而是定义模型类, 通过`模型类和对象`完成数据库表的`增删改查`.
- `ORM框架`就是把数据库表的行与相应的对象建立关联, 互相转换.使得数据库的操作面向对象.

## mysql配置 ##

**1. 在mysql中创建一个项目要使用的数据库**

```mysql
create database bookdb charset=utf8;
```

**2. setting里修改为mysql配置**

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'HOST': '127.0.0.1',  # 数据库主机(默认)
        'PORT': 3306,  # 数据库端口
        'USER': '你的数据库用户名',  # 数据库用户名
        'PASSWORD': '你的密码',  # 数据库用户密码
        'NAME': 'bookdb'  # 数据库名字
    }
}
```

**3. 在跟目录下配置pymysql**

```mysql
pip install pymysql
```

**4. 在子应用的 __ init __.py 文件中配置pymysql**

```python
import pymysql

pymysql.install_as_MySQLdb()
```

#### 模型迁移 （建表） ####

**生成迁移文件**：根据模型类生成创建表的语句

```py
python manage.py makemigrations
```

**执行迁移**：同步数据库

```py
python manage.py migrate
```

## 站点管理 ##

- **站点**: 分为`内容发布`和`公共访问`两部分
- **内容发布**的部分由网站的管理员负责查看、添加、修改、删除数据
- `Django`能够根据定义的模型类自动地生成管理模块
- 使用 `Django` 的管理模块, 需要按照如下步骤操作 :

#### 1. 管理界面本地化 ####

**本地化是将显示的语言、时间等使用本地的习惯，这里的本地化就是进行中国化.**

![界面本地化](https://i.loli.net/2021/11/26/869B2uRJpf3Krs4.png)

#### 2. 创建管理员 ####

**进入虚拟环境后**

创建管理员的命令 :

```django
python manage.py createsuperuser
```

![创建管理员](https://i.loli.net/2021/11/26/fuCmwZr5jaQphiM.png)

重置密码命令

```django
python manager.py changepassword 用户名
```

登陆站点 :`http://127.0.0.1:8000/admin`

输入你的用户名与密码，即可进行编辑

#### 3. 注册模型类 ####

在`子应用`的`admin.py`文件中注册模型类

需要导入模型模块 :`from 你的子应用.models import 要导入的表`；

这里演示为子应用book.models中导入BookInfo与PeopleInfo表，例如图：

![image.png](https://i.loli.net/2021/11/26/k2cGJH7sy3aAKit.png)

注册后：

![image.png](https://i.loli.net/2021/11/26/ubDkIxVqf6tOC4Q.png)

**注册完成后可对其表进行`添加数据` `修改数据` `删除数据`功能**

#### 4.发布内容到数据库 ####

前文连接数据库后，在后台添加的数据可直接在数据库查看以及修改，两者是相互对应的

```python
def __str__(self):
     """将模型类以字符串的方式输出"""
     return self.name
```

视图 与 URL

**用户在URL中请求的是视图，视图接收请求后进行处理 并将处理的结果返回给请求者.**

#### 视图 view ####

- 视图就是一个`Python`函数，被定义在`应用`的`views.py`中.
- 视图的第一个参数是`HttpRequest`类型的对象`reqeust`，包含了所有`请求信息`.
- 视图必须返回`HttpResponse对象`，包含返回给请求者的`响应信息`.
- 需要导入`HttpResponse`模块 :`from django.http import HttpResponse`

这里演示为子应用中的**migrations文件下的views.py**中创建发送请求函数，例如图：

![image.png](https://i.loli.net/2021/11/26/7k3S6CfynXZV2pv.png)

#### URLconf ####

**查找视图的过程 :**

- 1.请求者在浏览器地址栏中输入URL, 请求到网站.
- 2.网站获取URL信息.
- 3.然后与编写好的URLconf逐条匹配.
- 4.如果匹配成功则调用对应的视图.
- 5.如果所有的URLconf都没有匹配成功.则返回404错误.

`URLconf`入口

![image.png](https://i.loli.net/2021/11/26/Sae64i8yToLlQYk.png)

需要两步完成`URLconf`配置

- 1.在`项目`中定义`URLconf`

![image.png](https://i.loli.net/2021/11/26/et4Lh6c7GNjRTQB.png)

- 2.在`应用`中定义`URLconf`

![image.png](https://i.loli.net/2021/11/26/Tls1S6wIHzrJAuh.png)

**URL匹配全过程**

![image.png](https://i.loli.net/2021/11/26/HogW8E7uTrDvfdS.png)

## 模板 ##

**模板是一个文本，用于分离文档的表现形式和内容。**

### 模板使用步骤 ###

- 1.创建模板
- 2.设置模板查找路径
- 3.模板接收视图传入的数据
- 4.模板处理数据

#### 1.创建模板 ####

- 在`应用`同级目录下创建模板文件夹`templates`. 文件夹名称固定写法.
- 在`templates`文件夹下, 创建`应用`同名文件夹. 例, `Book`
- 在`应用`同名文件夹下创建`网页模板`文件. 例 :`index.html`

如图例：

![image.png](https://i.loli.net/2021/11/26/l8ZamhijC3MuqAS.png)

#### 2.设置模板查找路径 ####

![image.png](https://i.loli.net/2021/11/26/LtY71P8QESeBywl.png)

#### 3.模板接收视图传入的数据 ####

![image.png](https://i.loli.net/2021/11/26/1UQkmuV7cYAnE4R.png)

#### 4.模板处理数据 ####

![image.png](https://i.loli.net/2021/11/26/XgaHipjCoITVUR7.png)

#### 5. 查看模板处理数据成果 ####

![查看模板处理数据成果](https://i.loli.net/2021/11/29/2bcrRChynk1Le6q.png)

## 配置文件 ##

####  DEBUG ####

调试模式，创建工程后初始值为**True**，即默认工作在调试模式下。

- 修改代码文件，程序自动重启
- Django程序出现异常时，向前端显示详细的错误追踪信息

- 而非调试模式下，仅返回Server Error (500)

`注意：部署线上运行的Django不要运行在调式模式下，记得修改DEBUG=False和ALLOW_HOSTS。`

#### 静态文件 ####

项目中的CSS、图片、js都是静态文件。一般会将静态文件放到一个单独的目录中，以方便管理。在html页面中调用时，也需要指定静态文件的路径，Django中提供了一种解析的方式配置静态文件路径。静态文件可以放在项目根目录下，也可以放在应用的目录下，由于有些静态文件在项目中是通用的，所以推荐放在项目的根目录下，方便管理。

为了提供静态文件，需要配置两个参数：

- **STATICFILES_DIRS**存放查找静态文件的目录
- **STATIC_URL**访问静态文件的URL前缀

1） 在**项目根目录下创建static目录**来保存静态文件。

2） 在**bookmanager/settings.py**中修改静态文件的两个参数为

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]
```

3）此时在static添加的任何静态文件都可以使用网址**/static/文件在static中的路径**来访问了。

例如，我们向static目录中添加一个index.html文件，在浏览器中就可以使用127.0.0.1:8000/static/index.html来访问。

或者我们在static目录中添加了一个子目录和文件book/detail.html，在浏览器中就可以使用127.0.0.1:8000/static/book/detail.html来访问。

#### App应用配置 ####

在每个应用目录中都包含了apps.py文件，用于保存该应用的相关信息。

在创建应用时，Django会向apps.py文件中写入一个该应用的配置类，如

```python
from django.apps import AppConfig


class BookConfig(AppConfig):
    name = 'book'
```

我们将此类添加到工程settings.py中的INSTALLED_APPS列表中，表明注册安装具备此配置属性的应用。

- **AppConfig.name**属性表示这个配置类是加载到哪个应用的，每个配置类必须包含此属性，默认自动生成。

- **AppConfig.verbose_name**属性用于设置该应用的直观可读的名字，此名字在Django提供的Admin管理站点中会显示，如

  ```python
  from django.apps import AppConfig
  
  class UsersConfig(AppConfig):
      name = 'book'
      verbose_name
  ```


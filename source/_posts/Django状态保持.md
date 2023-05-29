---
title: Django状态保持
date: 2021-12-08 00:08:00
tag: Django
description: Django使用Cookie或Session进行状态保持
cover: https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto
---

![](https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto)

## 状态保持 ##

- 浏览器请求服务器是无状态的。
- **无状态**：指一次用户请求时，浏览器、服务器无法知道之前这个用户做过什么，每次请求都是一次新的请求。
- **无状态原因**：浏览器与服务器是使用Socket套接字进行通信的，服务器将请求结果返回给浏览器之后，会关闭当前的Socket连接，而且服务器也会在处理页面完毕之后销毁页面对象。
- 有时需要保持下来用户浏览的状态，比如用户是否登录过，浏览过哪些商品等
- 实现状态保持主要有两种方式：
  - 在客户端存储信息使用`Cookie`
  - 在服务器端存储信息使用`Session`

### Cookie ###

##### Cookie的工作原理 #####

由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？

**就给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。**

##### Cookie的特点 #####

- Cookie是由服务器生成，存储在浏览器端的一小段文本信息，以键值对方式进行存储。
- 通过浏览器访问一个网站时，会将本地存储的跟网站相关的所有cookie信息发送给该网站的服务器。
- Cookie是基于域名安全的。
- Cookie是有过期时间的，如果不指定，默认关闭浏览器之后cookie就会过期。

##### Cookie与django服务器执行流程 #####

![Cookie](https://s3.bmp.ovh/imgs/2021/12/9dc7cbce7fa0bf23.png)

#### 配置Cookie ####

通过 `HttpResponse` 对象中的 **set_cookie** 方法来设置cookie。

```python
HttpResponsse.set_cookit(sookie名, value=cookie值, max_age=cookie有效期)
```

写法

```python
def cookie(request):
    response = HttpResponse('ok')
    response.set_cookie('make'， 'Golang')  # 临时cookie
    response.set_cookie('luxor'， 'PHP', max_age=3600)  # 有效期一小时
    # max_age 单位为秒, 默认为None. 如果是临时cookie, 可将max_age设置为None.
```

#### 读取Cookie ####

可以通过 **HttpResponse** 对象的 **COOKIES** 属性来读取本次请求携带的cookie值。**request.COOKIES为字典类型**。

```python
def cookie(request):
    cookie1 = request.COOKIES.get('make')
    print(cookie1)
    return HttpResponse('OK')
```

### Session ###

Django完全支持也匿名会话，简单说就是使用跨网页之间可以进行通讯，比如显示用户名，用户是否已经发表评论。session框架让你 **存储和获取访问者的数据信息** ，这些信息保存在服务器上（默认是数据库中），以 **cookies 的方式发送和获取**一个包含 session ID的值，并不是用cookies传递数据本身。

##### Session的特点： #####

- 在服务器端进行状态保持的方案就是Session。
- session是以键值对进行存储的。
- session依赖于cookie。
- session也是有过期时间，如果不指定，默认两周就会过期。

##### Session与django服务器执行流程 #####

![Session与django服务器执行流程](https://s3.bmp.ovh/imgs/2021/12/faa7b58fed1e25b4.png)

#### 启用Session ####

**编辑 `settings.py` 中的一些配置**

**MIDDLEWARE_CLASSES** 确保其中包含以下内容

```python
'django.contrib.sessions.middleware.SessionMiddleware',
```

##### 数据库 #####

存储在数据库中，如下设置可以写，也可以不写，**这是默认存储方式**。

```python
SESSION_ENGINE='django.contrib.sessions.backends.db'
```

如果存储在数据库中，需要在项 **INSTALLED_APPS** 中安装Session应用。

```python
'django.contrib.sessions',
```

**这些是默认启用的。如果你不用的话，也可以关掉这个以节省一点服务器的开销。**

*数据库中的表如图所示*

![](https://s3.bmp.ovh/imgs/2021/12/72d8ea7f0c128e71.png)

由表结构可知，操作Session包括三个数据：键，值，过期时间。

##### 本地缓存 #####

存储在本机内存中，如果丢失则不能找回，比数据库的方式读写更快。

```python
SESSION_ENGINE='django.contrib.sessions.backends.cache'
```

##### 混合存储 #####

优先从本机内存中存取，如果没有则从数据库中存取。

```python
SESSION_ENGINE='django.contrib.sessions.backends.cached_db'
```

#### session使用 ####

1. 创建模拟登录视图

   ```python
   def testsession(request):
       # 更新数据库的session数据
       request.session['name'] = 'Shrimps'
       request.session['age'] = 22
       request.session['userid'] = 1024
       
       return HttpResponse('Good')
   ```

   

2. 创建模拟主页视图

   ```python
   from django.http import HttpResponse
   
   def testIndex(request):
   # 查询主页的数据
       userid = request.session.get('userid')
       name = request.session.get('name')
   
       if userid:
           print('登陆过')
           return HttpResponse(f'Hello - {name} ')
       else:
           print('未登录')
           return HttpResponse('未登录')
   ```

3. 登录后访问主页

   ![](https://s3.bmp.ovh/imgs/2021/12/e391b18e38622c4f.png)

> 在这里我是定义时间事件 所有才会显示晚上好

```python
# 代码如下 - (在 return HttpResponse('Good') 之前执行)

# 判断当前时间
    now_time = datetime.datetime.now().strftime('%H')
    now_time = int(now_time)
    if now_time > 12 and now_time < 18:
        now_time = '下午好'
    elif now_time < 12:
        now_time = '早上好'
    else:
        now_time = '晚上好'
```



#### Session操作 ####

通过HttpRequest对象的session属性进行会话的读写操作。

1） 以键值对的格式写session。

```python
request.session['键']=值
```

2）根据键读取值。

```python
request.session.get('键',默认值)
```

3）清除所有session，在存储中删除值部分。

```python
request.session.clear()
```

4）清除session数据，在存储中删除session的整条数据。

```python
request.session.flush()
```

5）删除session中的指定键及值，在存储中只删除某个键及对应的值。

```python
del request.session['键']
```

6）设置session的有效期

```python
request.session.set_expiry(value)
```

**value规则：**

- 如果value是一个整数，session将在value秒没有活动后过期。
- 如果value为0，那么用户 session的Cookie将在用户的浏览器关闭时过期。
- 如果value为None，那么session有效期将采用系统默认值， **默认为两周**，可以通过在 `settings.py` 中设置**SESSION_COOKIE_AGE**来设置全局默认值。


---
title: Django视图
date: 2021-12-05 00:08:00
tag: Django
description: Django视图配置应用与Http
cover: https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto
---

![](https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto)

## 视图定义 ##

- 一个视图函数，简称视图，就是`应用`中`views.py`文件中的函数，它接受 Web 请求并且返回 Web 响应。

- 视图必须返回一个`HttpResponse对象`或`子对象`（`JsonResponse`  `HttpResponseRedirect`）作为响应

- 视图负责接受Web请求`HttpRequest`，进行逻辑处理，返回Web响应`HttpResponse`给请求者

- 视图层中有两个重要的对象：**请求对象(request)**与 **响应对象(`HttpResponse`)**。
- 处理过程：

![视图处理过程](https://i.loli.net/2021/12/03/75NXDBbUJtesIPn.png)

## 视图使用 ##

1. 在项目的`urls.py`文件定位应用的`urls.py`

   ```python
   from django.contrib import admin # 默认配置
   from django.urls import path, include # 导入include
   
   urlpatterns = [
       path('admin/', admin.site.urls), # 默认配置
       path('', include('book.urls')) # 配置应用的urls
   ]
   
   ```

   

2. 在应用的`views.py`中定义视图函数

   ```python
   from django.http import HttpResponse
   
   # 这里定义index视图函数 向页面传递完成请求
   def index(request): # request为请求的数据
       print('测OwO试')
       return HttpResponse('OK')
   
   ''' 也可以通过render映射到templates指定html中 例如：'''
   from django.shortcuts import render 
   def index(request)：
   	data = {'text':'测OwO试'}
   	return render(request,'book/index.html',data)
   ```

   

3. 在应用的`urls.py`中配置视图函数

   ```python
   from django.urls import path
   from book.views import index # 从你的应用.views中导入视图函数
   
   # 固定写法 urlpatterns = [...]
   urlpatterns = [
       # 通过path('地址', 视图函数) 导入
       path('index/', index),
   ]
   ```

## HttpRequest对象 ##

Django 使用请求和响应对象通过系统传递状态。

当请求一个页面时，Django 创建一个[`HttpRequest`](https://docs.djangoproject.com/en/3.0/ref/request-response/#django.http.HttpRequest)包含请求元数据的对象。然后 Django 加载适当的视图，将 传递[`HttpRequest`](https://docs.djangoproject.com/en/3.0/ref/request-response/#django.http.HttpRequest)给视图函数的第一个参数。每个视图负责返回一个[`HttpResponse`](https://docs.djangoproject.com/en/3.0/ref/request-response/#django.http.HttpResponse)对象。

#### 请求体 ####

请求体数据格式不固定，可以是表单类型字符串 或 JSON字符串 或 XML字符串，要区别对待。

可以发送请求体数据的请求方式有**POST**、**PUT**、**PATCH**、**DELETE**。

**Django默认开启了CSRF防护**，会对上述请求方式进行CSRF防护验证，在测试时可以关闭CSRF防护机制，方法为在项目中的settings.py文件中注释掉CSRF中间件

![请求体关闭防护验证](https://s2.loli.net/2021/12/06/1hGSQLNyMU8jbWu.png)

#### QueryDict ####

**数据类型是 QueryDict，一个类似于字典的对象，包含 HTTP GET 的所有参数。**

##### `GET` #####

有相同的键，就把所有的值放到对应的列表里。

取值格式：**对象.方法**。

**get()**：返回字符串，如果该键对应有多个值，取出该键的最后一个值。

`不存在返回None`,可以设置默认值进行后续处理

```python
get('键', 默认值)
request.GET.get()
```

**getlist()：**根据键获取值，值以列表返回，可以获取指定键的所有值

`不存在返回空列表`,可以设置默认值进行后续处理

```python
getlist('键', 默认值)
request.GET.getlist()
```

##### `POST` #####

常用于 form 表单，form 表单里的标签 name 属性对应参数的键，value 属性对应参数的值。

取值格式： **对象.方法**。

**get()**：返回字符串，如果该键对应有多个值，取出该键的最后一个值。

```python
request.POST.get()

# 可在 Postman 中传递参数 发送post请求
```

#### Query String ####

**获取请求路径中的查询字符串参数，可以通过request.GET属性获取，返回QueryDict对象。**

```python
# 语法 request.GET.get/getlist(xxx)
# 如： /get/?a=333&b=1024

def get(request):
    a = request.GET.get('a')
	b = request.GET.get('b')
	print(a , b) # 333, 1024
	return HttpResponse('OK)
```

#### HttpRequest常用对象 ####

##### body #####

数据类型是二进制字节流，是原生请求体里的参数内容，在 HTTP 中用于 POST，因为 GET 没有请求体，**返回bytes类型**。

在 HTTP 中不常用，而在处理非 HTTP 形式的报文时非常有用，例如：二进制图片、XML、Json 等。

```python
xx = request.body
print(xx) # 二进制的形式打印
```

##### path #####

获取 URL 中的路径部分，数据类型是字符串

```python
xx = request.path
print(xx） # 路径 如：/book/ 
```

##### method #####

表示请求使用的HTTP方法，数据类型是字符串，且结果为大写（常用值包括：'GET'、'POST' ）

```python
xx = request.method
print(xx) # 请求方式 如: POST
```

##### encoding #####

一个字符串，表示提交的数据的编码方式。

如果为None则表示使用浏览器的默认设置，一般为`utf-8`。

这个属性是可写的，可以通过修改它来修改访问表单数据使用的编码，接下来对属性的任何访问将使用新的encoding值。

```python
xx = request.encoding
print(xx) # None(默认设置<UTF8>)
```

##### FILES #####

一个类似于字典的对象，包含上传的文件。

```python
xx = request.FILES
print(xx) # <MultiValueDict: {}> (包含所有的上传文件)
```

##### COOKIES #####

一个标准的Python字典，包含所有的cookie，键和值都为字符串。

```python
xx = request.COOKIES
print(xx) # {} (默认为{})
```

##### session #####

一个既可读又可写的类似于字典的对象，表示当前的会话，只有当Django 启用会话的支持时才可用，详细内容见"状态保持"。

```python
xx = request.session
print(xx) # <django.contrib.sessions.backends.db.SessionStore object at 0x7f42da71beb8>
```

#### From Data 表单类型 ####

前端发送的表单类型的请求体数据，可以通过request.POST属性获取，返回 **QueryDict对象**。

```python
# 语法 request.POST.get/getlist(xxx)

def post(request):
    a = request.POST.get('a')
    aa = request.POST.getlist('a')
    print(a, b ) 
    return HttpResPonse('OK')
```

#### Non-Form Data 非表单类型 ####

非表单类型的请求体数据，Django无法自动解析，可以通过**request.body**属性获取最原始的请求体数据，自己按照请求体格式（JSON、XML等）进行解析。**request.body返回bytes类型(二进制格式)。**

```python
# 如数据 ： {'a': 1, 'b': 2}

# 1. 导包
import json

# 2. 解析程序
def post_json(request):
	json_str = request.body # 转换二进制格式
    json_str = json_str.decode() # 编码 # python3.6 无需执行此步
    req_data = json.loads(json_str) # 将json格式数据转换为字典
    print(req_data['a'], req_data['b'])
    
    return HttpResponse('OK')
```

#### 路径转换器 ####

系统为我们提供了一些路由转换器位置在`django.urls.converters.py`

```python
DEFAULT_CONVERTERS = {
    'int': IntConverter(), # 匹配正整数，包含0
    'path': PathConverter(), # 匹配任何非空字符串，包含了路径分隔符
    'slug': SlugConverter(), # 匹配字母、数字以及横杠、下划线组成的字符串
    'str': StringConverter(), # 匹配除了路径分隔符（/）之外的非空字符串，这是默认的形式
    'uuid': UUIDConverter(), # 匹配格式化的uuid，如 075194d3-6885-417e-a8a8-6c931e272f00
}
```

我们可以通过以下形式来验证数据的类型

```python
 path('<int:id>/<slug:username>/', login),
```

#### 自定义转换器 ####

如果默认的路由转换器无法满足需求时，我们就需要 **自定义路由转换器。**

在任意可以被导入的python文件中，都可以自定义路由转换器。

我们这里给他划分为5步：

1. 创建一个converters.py，在文件中定义一个类。
2. 在类中定义一个属性regex，这个属性是用来保存url转换器规则的正则表达式。
3. 实现to_python(self,value)方法，这个方法是先将url中的值转换，再传给视图函数的。
4. 实现to_url(self,value)方法，这个方法是在做url反转时，将传进来的参数转换后拼接成一个正确的url。
5. 将定义好的转换器，注册到django中。

示例<三步曲>  定义一个为匹配手机号的自定义转换器  ：

- **第一步**  项目根目录下，新建 `converters.py` 文件，用于自定义路由转换器

```python
"""自定义路由转换器：匹配手机号"""

class MobileConverter:

  # 匹配手机号码的正则
  regex = '1[3-9]\d{9}'

  def to_python(self, value):
      # 将匹配结果传递到视图内部时使用
      return int(value)

  def to_url(self, value):
      # 将匹配结果用于反向解析传值时使用
      return str(value)
```

- **第二步**  在`总路由`中注册自定义路由转换器

```python
from django.urls import register_converter
# 注册自定义路由转换器
# 语法: register_converter(自定义路由转换器,'别名')
register_converter(MobileConverter, 'mobile')

urlpatterns = []
```

- **第三步** 使用自定义路由转换器

```python
# 子路由使用

path('<mobile:phone>/', register)

'''测试path()中自定义路由转换器提取路径参数：手机号(http://xxx.x.xx.x:8000/18500001111/)'''
```

#### 请求头 ####

可以通过 `request.META` 属性获取请求头headers中的数据，**request.META为字典类型**。

包含所有可用 HTTP 请求头的字典。可用的请求头取决于客户端和服务器，

| 常见的请求头         | 说明                                   |
| -------------------- | -------------------------------------- |
| CONTENT_LENGTH       | 请求正文的长度（作为字符串）           |
| CONTENT_TYPE         | 请求正文的 MIME 类型                   |
| HTTP_ACCEPT          | 可接受的响应内容类型                   |
| HTTP_ACCEPT_ENCODING | 可接受的响应编码                       |
| HTTP_ACCEPT_LANGUAGE | 可接受的响应语言                       |
| HTTP_HOST            | 客户端发送的 HTTP Host 头              |
| HTTP_REFERER         | 引用页面 ( 如果有的话 ）               |
| HTTP_USER_AGENT      | 客户端的用户代理字符串                 |
| QUERY_STRING         | 查询字符串，作为单个（未解析的）字符串 |
| REMOTE_ADDR          | 客户端的IP地址                         |
| REMOTE_HOST          | 客户端的主机名                         |
| REMOTE_USER          | Web 服务器验证的用户（如果有）         |
| REQUEST_METHOD       | 一个字符串，例如`"GET"`or `"POST"`     |
| SERVER_NAME          | 服务器的主机名                         |
| SERVER_PORT          | 服务器的端口（作为字符串）             |

#### 使用JSON传递数据 ####

**路由**

使用应用`urls.py`视图函数

```python
# 语法：path('路径/',视图函数)
path('index/', index)
```

**视图**

定义`Json`视图函数

```python
def index(request):
    # 1. 通过 request.body 获取json数据 (为bytes类型的二进制数据)
    json_str = request.body
    
    # 2. 通过 decode 编码转为json字符串
    json_str = json_str.decode()
    print(json_str, type(json_str))
    
    # 3. 把json字符串转换为字典
    data_dict = json.loads(json_str)
    
    name = data_dict.get('name')
    user = data_dict.get('user')
    password = data_dict.get('password')
    
    return HttpResoponse(f'{name} - {user} - {password}')
```

**Postman**

使用Postman模拟传递数据

![Postman](https://i.loli.net/2021/12/03/6FoMiPtB8hD7xIR.png)

## HttpResPonse对象 ##

视图在接收请求并处理后，**必须返回HttpResponse对象或子对象**。

HttpRequest对象由Django创建，HttpResponse对象由开发人员创建。

#### HttpResponse ####

使用**django.http.HttpResponse**来构造响应对象

```python
HttpResponse(content=响应体, content_type=响应体数据类型, status=状态码)
```

也可通过HttpResponse对象属性来设置响应体、响应体数据类型、状态码；

响应头可以直接将HttpResponse对象当做字典进行响应头键值对的设置。

```python
from django.http import HttpResponse  # 导包

def test(request):
    # #content 是响应给客户端的数据,status指的是状态
    return HttpResponse('测试一下 O-O ', status= 400)

    # or
    
    response = HttpResponse('我是content哦')  #响应体
	response.status_code = 400  # 状态码
    response['data'] = "I'm a String ,QWQ"  # 响应头
    return response
```

####  HttpResponse子类 ####

Django提供了一系列HttpResponse的子类，可以快速设置状态码，这些类在 django.http 中。

| More Actions类名              | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| HttpResponseRedirect          | 构造函数的参数有一个：重定向的路径。 它可以是一个完整的URL（例如， 'http:www.baidu.com' ）或者不包括域名的绝对路径（如 '/search/' ）。 注意它返回 HTTP 状态码 302。 |
| HttpResponsePermanentRedirect | 类似 HttpResponseRedirect ， 但是它返回一个永久转义 （HTTP状态码 301），而不是暂时性转移（状态码302）。 |
| HttpResponseNotModified       | 构造函数没有任何参数。用它来表示这个页面在上次请求后未改变。 |
| HttpResponseBadRequest        | 类似 HttpResponse ，但使用400状态码。                        |
| HttpResponseNotFound          | 类似 HttpResponse ，但使用404状态码。                        |
| HttpResponseForbidden         | 类似 HttpResponse ，但使用403状态码。                        |
| HttpResponseNotAllowed        | 类似 HttpResponse ，但使用405状态码。它必须有一个参数：允许方法的列表。（例如， ['GET', 'POST'] ）。 |
| HttpResponseGone              | 类似 HttpResponse ，但使用410状态码。                        |
| HttpResponseServerError       | 类似 HttpResponse ，但使用500状态码。                        |

#### JsonResponse ####

**若要返回json数据，可以使用JsonResponse来构造响应对象**

作用：帮助我们将数据转换为json字符串

设置响应头**Content-Type**为  **`application/json`**

实例使用：

1. 在视图中   创建json数据

   ```python
   from django.http import JsonResponse  # 导包
   
   def responseJson(request):
       # json数据
       datalist = {'id': 1024, 'name': 'Shrimps', 'password': 123456, 'job': 'Front-end engineer'}
       # 返回json数据
       return JsonResponse(datalist)
   ```

   

2. 在路由中   引入json

   ```python
   from 应用名.views import responseJson  # 引用
   
   urlpatterns = [
       path('jsonTest/', responseJson)  # 使用
   ]
   ```

3. 页面显示效果

   ![页面显示效果](https://s3.bmp.ovh/imgs/2021/12/4dfeb20f27efcdcd.png)



### 类视图 ###

以函数的形式进行定义的视图就是**函数视图**,视图函数便于理解,但是遇到一个视图函数对应的路径提供了多种不同的HTTP请求方式的支持时(get,post,delete,put),需要在一个函数中写不同的业务逻辑,代码的可读性和复用性就很底, 所以,我们引入类视图进行解决.

##### 类视图的优点: #####

- **代码可读性好**
- **类视图相对于函数视图有更高的复用性,如果其他地方需要使用到某个类的某个特定方法,直接继承该类的视图就可以了**

####  类视图的使用 ####

定义类视图需要继承自的[Django](https://so.csdn.net/so/search?from=pc_blog_highlight&q=Django)提供的父类的View:

- 导入方法：

```python
from django.views.generic import View    
# or
from django.views.generic.base import View
```

> 配置路由时,需要使用类视图的as_view()方法来注册添加

- 应用中 -  `views.py` 定义类视图

```python
# 类视图方法
class RegisterView(View):

    def get(self, request):
        print('跳转页面')
        name = {'username': request.session.get('name')}
        if not name['username']:
            name['username'] = '新用户'
        # 这里我是在模板templates下创建register.html文件 方便查看
        return render(request, 'register.html', name)

    def post(self, request):
        print('注册逻辑')
        return HttpResponse('post')

    def put(self, request):
        print('putput')
        return HttpResponse('put')

    def delete(self, request):
        print('delete')
        return HttpResponse('delete')
```
- 应用中 -  `urls.py` 注册路由


```python
from 应用名.views import RegisterView

# 类视图 - 注册路由
path('register/', RegisterView.as_view())
```
- templates中  - `register.html` 映射到页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title> OwO </title>
</head>
<body>
    <h1>你好 - {{ username }}</h1>
</body>
</html>
```




















---
title: Django 跨域CORS
date: 2021-12-28 00:08:00
tag: Django
description: 解决Django前后端跨域问题
cover: https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto
---

![](https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto)

## 跨域CORS ##

我们的前端和后端分别是两个不同的端口

这里用 www.baidu.com 举例子 改变的是端口

| 位置     | 域名               |
| :------- | :----------------- |
| 前端服务 | www.baidu.com:8080 |
| 后端服务 | www.baidu.com:8000 |

**现在，前端与后端分处不同的域名，这就涉及到跨域访问数据的问题，因为浏览器的同源策略，默认是不支持两个不同域间相互访问数据，而我们需要在两个域名间相互传递数据，这时我们就要为后端添加跨域访问的支持。**

> 我们使用CORS来解决后端对跨域访问的支持。

使用 [django-cors-headers ](https://github.com/ottoyiu/django-cors-headers/) 扩展  

#### 安装 ####

```python
pip install django-cors-headers
```

#### 添加应用 ####

```python
INSTALLED_APPS = (
    ...
    'corsheaders',
    ...
)
```

#### 中间层设置 ####

```python
MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
    ...
]
```

#### 添加白名单 ####

```python
# CORS
CORS_ORIGIN_WHITELIST = (
    'http://127.0.0.1:8080', # 为本地页面 8080端口
    'http://www.baidu.com:8080', # 为前端页面 8080 端口
)
CORS_ALLOW_CREDENTIALS = True  # 允许携带cookie
```

- 凡是出现在白名单中的域名，都可以访问后端接口
- CORS_ALLOW_CREDENTIALS 指明在跨域访问中，后端是否支持对cookie的操作。
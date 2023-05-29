---
title: Axios语法
date: 2021-09-11 00:00:00
tag: 前端技术
description: 
cover: https://s3.bmp.ovh/imgs/2021/12/8e7a6a989a3cf828.png
---


# AXIOS #

`Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中，向服务器发送AJAX请求进行数据交换`

------

## 安装 ##

使用 npm:

```javascript
$ npm install axios
```

使用 bower:

```javascript
$ bower install axios
```

使用 cdn:

```javascript
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.min.js"></script>
```

## 基本使用 ##

##### *默认发送* #####

```js
    <script>
        axios({url:'https://www.fastmock.site/mock/a62b6007be40808b2edb8a8a3fb54c6a/api/book'})
            .then(res =>{ console.log(res.data.data.list);})
    </script>
```

##### *使用get发送无参请求* #####

```js
    <script>    axios({url:'https://www.fastmock.site/mock/a62b6007be40808b2edb8a8a3fb54c6a/api/book',
                method:'get'})
            .then(res =>{ console.log(res.data.data.list);})
    </script>
```

##### *通过axios使用get发送有参请求* #####

```js
// 1.url地址后添加格式 ？查询键:'键值'
    <script>  axios({url:'https://www.fastmock.site/mock/a62b6007be40808b2edb8a8a3fb54c6a/api/book?id:999', })
        .then(res=>console.log(res))
    </script>
// 2.通过axios内置参数 params
<script>   axios({url:'https://www.fastmock.site/mock/a62b6007be40808b2edb8a8a3fb54c6a/api/book,
     params:{ id:1002 }
            }).then(res=>console.log(res))
    </script>
```

##### *使用post发送无参请求* #####

```js
<script>
 axios({url:'https://www.fastmock.site/mock/5130d2d2fbd00f0defbe3da6e10cd169/books/serach',
     method:'post'})
     .then(res =>{ console.log(res.data.data);})
    </script>
```

##### *使用axios发送post发送有参请求* #####

```js
<script>
        axios({url:'https://www.fastmock.site/mock/a62b6007be40808b2edb8a8a3fb54c6a/api/tests',
                method:'post',        
                data:'忽如寄'})
            .then(res =>{ console.log(res.data.data);})
    </script>
```

## axios基本使用 ##

##### 使用axios.get发送无参请求 #####

```js
        <script> axios.get('https://www.fastmock.site/mock/a62b6007be40808b2edb8a8a3fb54c6a/api/book')
                .then(res => console.log(res.data.desc))
                .catch(err => console.warn('失败了' +err))
        </script>
```

##### *使用axios.get发送有参请求* #####

```js
// get的第二个参数为对象 同时使用params查询的参数也要为对象形式
<script>
  axios.get('https://www.fastmock.site/mock/a62b6007be40808b2edb8a8a3fb54c6a/api/book',
                {params:{id:1002}})
            .then(res => console.log(res.data.desc))
            .catch(err => console.warn('失败了' +err))
    </script>
```

##### *使用axios.post发送有参请求*  **-- 只修改前端代码** #####

```js
// 使用post用这种方式比较安全，多个参数之间用 && 隔开
<script>
 axios.post('https://www.fastmock.site/mock/5130d2d2fbd00f0defbe3da6e10cd169/books/serach',
                    "id:1002&&bookname:'Web前端开发实战")
                .then(res => {console.log(res.data.data.list)})
                .catch(err => console.log(err))
    </script>
```

##### 使用data传递数据 后台需要将axios自动装换的json数据转换为java对象  **-- 修改后台代码** #####

```js
<script>
 axios.post('https://www.fastmock.site/mock/5130d2d2fbd00f0defbe3da6e10cd169/books/serach',{bookname:'Web前端开发实战'})
                .then(res => {console.log(res.data.data.list)})
                .catch(err => console.log(err))
    </script>
` 接收参数添加 @requestBody 注解`
```

## axios并发请求 ##

```js
// 并发同时返回多个接口参数
    <script>
        axios.all([
            axios.get('https://www.escook.cn/api/cart'),
            axios.get('https://www.escook.cn/api/cart', { params: { id: 2 } }),
        ])
            .then(res => console.log(res[0], res[1]))
            .catch(err => console.log(err))
    </script>

// 在then中使用 axios.spread 方法，可让返回的数据分开易看
    <script>
        axios.all([
            axios.get('https://www.escook.cn/api/cart'),
            axios.get('https://www.escook.cn/api/cart', { params: { id: 2 } }),
        ])
            .then(
                axios.spread((res1, res2) => {
                    console.log(res1);
                    console.log(res2);
                })
            ).catch(err => console.log(err))
    </script>
```

## axios全局配置 ##

```js
    <script>
        // 配置全局属性
        axios.defaults.baseURL = "https://www.escook.cn/api";
		// 设置超时返回指定
        axios.defaults.timeout = 5000;
		
		// 在全局配置的基础上进行网络请求 -> get
        axios.get('cart')
            .then(res=>console.log(res))
		// 在全局配置的基础上进行网络请求 -> post
        axios.post('***')
            .then(res=>console.log(res))
            .catch(err=>console.log(err))
    </script>
```

## axios实例 ##

```js
    <script>
        // 创建axios实例
        let newVar = axios.create({
            baseURL:'https://www.escook.cn/api',
            timeout: 5000
        })
        newVar({
            url:'cart'
        }).then(res=>console.log(res))
    </script>
```

## axios拦截器 ##

**拦截器的作用：用于我们在网络请求的时候发起请求或者响应时对操作进行响应的处理**

场景：发起请求时可以添加页面加载的动画 强制登录 响应时进行相应的数据处理

axios提供了两大类拦截器

请求方向的拦截( 成功请求，失败请求 ) 

```js
    <script>
        axios.interceptors.request.use(config => {
            console.log('进入拦截器');
            console.log(config);
            return config; // 放行请求
        }),err=>{
            console.warn('请求拦截失败');
            console.warn(err);
        }

        axios.get('https://www.escook.cn/api/cart')
            .then(res=>console.log(res))
    </script>
```

请求响应的拦截( 成功的, 失败的)

```js
    <script>
        axios.interceptors.response.use(config => {
            console.log('进入响应截器');
            return config.data.list;
        }), err => {
            console.warn('响应方向失败');
        }

        axios.get('https://www.escook.cn/api/cart')
            .then(res => console.log(res))
    </script>
```

## axios在vue中的模块封装 ##

```js
// 1. 通过封装者配置

`封装的位置` 
import axios from 'axios'

export function request(config, success, fail) {
    axios({
        url: config
    }).then(res => {
        success(res)
    }).catch(err => {
        fail(err)
    })
}

`调用的位置`
import { request } from "./network/request";
request('https://www.escook.cn/api/cart',
  res => console.log(res),
  err => console.log(err),
)
```

```js
// 2.通过调用者配置

`封装的位置` 
import axios from 'axios'
export function request(config) {
    axios.defaults.baseURL = "https://www.escook.cn/api/cart"
    axios(config.url)
        .then(
            res => config.success(res)
        )
        .catch(
            err => config.fail(err)
        )
}

`调用的位置`
import { request } from "./network/request";
request({
  url: 'https://www.escook.cn/api/cart',
  success: res => { console.log(res) },
  fail: err => { console.log(err) }
})
```

  ```js
// 3.使用Promise配置

`封装的位置` 
import axios from 'axios'
export function request(config){
    return new Promise((resolve,reject) => {
        let newVar = axios.create({
            baseURL: 'https://www.escook.cn/api',
            timeout: 5000
        });
        newVar(config).then(res => {
            resolve(res);
        }).catch(err => {
            reject(err)
        })
    })
}

`调用的位置`
import { request } from './network/request'
request({
  url:'cart'
}).then(res => {console.log(res)
}).catch(err => console.log(err))
  ```

##### 终极方案<推荐> #####

```js
`封装的位置` 

import axios from 'axios'
export function request(config){
    let newVar = axios.create({
        baseURL:'https://www.escook.cn/api',
        timeout: 5000
    })
    return newVar(config)
}

`调用的位置`
import { request } from './network/request'
request({
  url: 'cart'
}).then(res => {
  console.log(res)
}).catch(err => console.log(err))
```


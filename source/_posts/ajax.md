---
title: AJAX
date: 2021-03-25 00:00:00
updated:
tag: 前端技术
description: 'AJAX精装笔记快速复习'
cover: https://s3.bmp.ovh/imgs/2021/12/e57b8469ed115292.png
---

# AJAX #

#### `概念`： ####

#### Asynchronous JavaScript And XML 异步的JavaSrcipt和XML ####

简说：Ajax通过在**后台与服务器进行少量数据交换**，使网页实现异步更新；

特点：可以实现**页面无刷新更新数据**，提高用户浏览网站应用的体验；

- 同步 or 异步

同步：客户端必须等待程序的响应；在等待的期间客户端不能做其他操作。

异步：客户端不需要等待程序的响应；在服务器处理请求的过程中，客户端可以进行其他的操作

## 实现方法 ##

#### 1.创建核心对象 ####

- 创建 `XMLHttpRequest` 对象

```javascript
var iable= new XMLHttpRequest();
//老版本的IE5~6 使用ActiveX 对象： if 上述用不了用下述
var iablet = new ActiveXObject('Microsoft.XMLHTTP')
```

## 2.建起连接 ##

- 步骤：向服务器发送请求 ，接收服务器响应，处理数据

1. 向服务器发送请求

```javaScript
Xmlhttp.open('Get','textl.txt',true);
//open(method,url,async);
//method:请求的类型：GET 或 POST
//url:文件在服务器上的位置
//async: true（异步）或false（同步）
xmlhttp.send();
//send(string);
//string:仅用于POST请求
```

## 3.请求方式 ##

- ##### GET与POST #####

get方法：请求参数在URL后边拼接。send方法为空参数

post方法：请求参数在send方法中定义

**GET更简单也更快，POST在以下情况下使用**

1. 无法使用缓存文件（更新服务器上的文件或数据库）
2. 向服务器发送大量数据（POST 没有数据量限制）
3. 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

- GET    请求方式

  ```javascript
  xhr.open('get', 'http://www.example.com?name=zhangsan&age=20');
  ```

- POST 请求方式

  ```javascript
  xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded') xhr.send('name=zhangsan&age=20');
  ```

  （POST要设置请求头：xhr.setRequestHeader）

  "Content-Type":作用是告知服务器，浏览器提交的数据类型

  值为：application/x-www-form-urlencoded（需要自己指定） ，表示客户端提交的是`查询字符串`。
  值为：application/json（需要自己指定） ，表示客户端提交的是 `JSON 字符串`。
  值为：multipart/form-data（浏览器会自动设置），表示客户端提交的是 `FormData 对象`。

  

  `GET`
  GET方法请求一个指定资源的表示形式. 使用GET的请求应该只被用于获取数据。
  通俗的讲，获取数据应该使用GET方式的请求。
  `POST`
  POST方法用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用。
  添加、修改都可以使用POST请求，因为此类操作都会改变服务器上的资源
  `PUT`
  PUT方法用请求有效载荷替换目标资源的所有当前表示。
  修改操作可以选择使用PUT方式
  `DELETE`
  DELETE方法删除指定的资源。
  删除操作可以使用DELETE方式

  

  ##### Ajax 状态码 #####

  在创建ajax对象，配置ajax对象，发送请求，以及接收完服务器端响应数据，这个过程中的每一个步骤都会对应一个数值，这个数值就是ajax状态码。

  ```
  0：请求未初始化(还没有调用open())
  1：请求已经建立，但是还没有发送(还没有调用send())
  2：请求已经发送
  3：请求正在处理中，通常响应中已经有部分数据可以用了
  4：响应已经完成，可以获取并使用服务器的响应了
  
  xhr.readyState // 获取Ajax 状态码
  ```

  #### status属性 ####

  status 属性表示 http 状态码，是一个数字，代码指示特定 HTTP 请求是否已成功完成。
  响应分为五类：
  信息响应(100–199)，
  成功响应(200–299)，
  重定向(300–399)，
  客户端错误(400–499)
  服务器错误 (500–599)。
  常用状态码
  200 OK  -  请求成功
  400 Bad Request  -  1、语义有误，当前请求无法被服务器理解。2、请求参数有误。
  401 Unauthorized  -  当前请求需要用户验证。
  404 Not Found  -  请求失败，请求所希望得到的资源未被在服务器上发现。
  500 Internal Server Error  -  服务器遇到了不知道如何处理的情况。

  ##### onreadystatechange 事件 #####

  当 Ajax 状态码发生变化时将自动触发该事件

  在事件处理函数中可以获取 Ajax 状态码并对其进行判断，当状态码为 4 时就可以通过 xhr.responseText 获取服务器端的响应数据了.

  ```javascript
   // 当Ajax状态码发生变化时
   xhr.onreadystatechange = function () {
       // 判断当Ajax状态码为4时
       if (xhr.readyState == 4) {
           // 获取服务器端的响应数据
           console.log(xhr.responseText);
       }
   }
  ```

  ##### 原生JS实现请求参考代码： #####

  ```javascript
  // 1.创建xhr对象
  let xhr = new XMLHttpRequest();
  // 2.事件监听
  xhr.onreadystatechange = function(){
      //判断页面加载完成再执行
      if(xhr.readyState == 4){
          console.log(xhr.responseText);
      }
  }
  xhr.open('post','http://www.liulongbin.top:3006/api/addbook');//请求方式与请求地址
  xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');//请求头
  xhr.send('bookname=海子的诗&author=海子&publisher=自由与遐想出版社');//需要添加的值
  ```

  

#### 原生JS进行封装 ####

分析：

①：函数只有一个参数，是一个对象
对象里包括 type、url、data、success四个属性
②：GET和POST请求，都需要创建xhr对象，都需要设置 onreadystatechange 事件
③：当得到响应结果后，调用 success 函数，把结果传递给 success 函数
④：需要把对象形式的请求参数，转换成查询字符串
⑤：由于GET和POST的 “open”和 “send”不一样，所以判断，然后分开写
⑥：优化（默认GET、大小写等等）；

参考代码：

```javascript
<script>
        //获取的数据转换为字符串格式
        function toStr(data){
        var list = [];
        for(var key in data){
            list.push(`${key}=${data[key]}`);
        };
        return list.join('&');
        };
        //事件监听
        function ajax(option){
        let preat = toStr(option.data);                       //所有data数据
        let type = option.type;                               //获取方式类型         
        let xhr = new XMLHttpRequest();                       //固定创建XML
        xhr.onreadystatechange= function(){
            if(xhr.readyState == 4 && xhr.status == 200){     //页面完成时正确输出执行
                option.success(JSON.parse(xhr.responseText)); //回调数据转换为数组
            }};
        if(type = 'get'){                                     //判断方式类型
            xhr.open(type,`${option.url}?${preat}`);          //get是写在open()
            xhr.send();
        }else{
            xhr.open(type,oprion.url);
            xhr.send(preat);                                  //post写在send()
        }};
        // 构造函数实参
        ajax({
            url:'http://www.liulongbin.top:3006/api/getbooks', //请求地址
            type:'get',                                        //请求方式
            data:{                                             //添加数据
                id:5996,
                name:'zhangsan',
                age:22,
                IdPrice:888888
            },
            success:(res)=>console.log(res)                    //回调数据
        });
```



### 编码与解码 ###

- 乱码产生的原因:

所有浏览器提供的AJAX对象请求参数默认使用 UTF-8 进行编码；而服务器端默认使用 ISO-8859-1 去解码，编码与解码方式不一致导致了乱码的产生。

- 解决方式:

服务器端使用 request.setCharacterEncoding("UTF-8"); 来明确指明服务器端使用 UTF-8 进行解码;

```javascript
编码：encodeURIComponent('椒盐虾');  //编码： %E6%A4%92%E7%9B%90%E8%99%BE
```

```javascript
解码：decodeURIComponent(变量);  // 解码： 椒盐虾
```



## XHR2 ##

### 新增事件 ###

1. **请求超时**

timeout：用于设置请求的超时时间，单位是毫秒

ontimeout：如果请求超时了，会触发 ontimeout 事件

```JavaScript
xhr.timeout = 30;
xhr.ontimeout = function(){alert('请求超时，请刷新重试')}
```

2. **onload事件**

onload事件,在请求成功完成时触发；即 readyState为4时触发。

```javascript
//可使用onload事件替代之前的onreadystatechange事件
xhr.onload = function(){if(xhr.stuaus == 200){console.log(this.responseText);}}
```

  3.**onerror事件**，在请求失败时触发

```javascript
xhr.onerror = function(){console.log(请求失败了);}；
xhr.open('GET','http://xxxxxxxxxxx.com'); //错误地址
```

4. **onprogress事件**，获取数据接收速度，readyState为3时触发。

```javascript
xhr.onprogress = function (event) { 
    event.loaded;  // 已传输的数据量
    event.total;   //总共的数据量
};
```

5. **onloadstart事件** 与 **onloadend事件**

```javascript
xhr.onloadstart = function () {
    console.log('即将开始下载数据') //开始传输数据
};
xhr.onloadend = function () { 
    console.log('请求已经结束了');  // 已传输的数据量
};
```

#### 总结 ####

1. load，请求成功时触发

2. error，失败时触发

3. loadstart，开始传送数据时触发

4. loadend，请求完成（成功或失败）时触发

5. progress，分块接收大量数据时，周期性触发

6. timeout，超时后触发

## FormData ##

- FormData的作用和 jQuery中的 serialize() 作用一样，用于快速收集表单数据，并且可以将结果直接提交给接口。
  创建的FormData对象，可直接通过 xhr.send(FormData对象) 提交给服务器的接口

#### 语法： ####

```javascript
// 1. 获取 form 标签的 DOM对象
let form = document.querySelector('form');
// 2. 实例化 FormData 对象，传入 form
let fd = new FormData(form);
// 3. 其他代码  略
// 4. 提交数据
xhr.send(fd);
```

#### FromData注意事项： ####

- new FormData() 后，得到一个对象；
- form参数可选
- FormData 对象里包括所有的表单数据，但是无法通过打印对象查看，可以通过forEach方法遍历查看。
- 数据不需要转换成查询字符串格式，直接提交FormData对象即可
- 提交FormData对象，只能使用`POST方式`
- 提交FormData对象，对应的Content-Type由浏览器自动设置，不需要程序员自己设置

## J S O N ##

- JSON作为一种通用的格式 在传输数据时当做数据的载体

JSON的本质是`字符串`（如果在JS中写，一定要写成字符串形式）
JSON字符串，也是有结构的，样子和 JS 数据结构基本一样
JSON字符串中，允许出现的类型有 null number string bool array object
注释、函数、undefined等其他所有类型都不能出现在JSON字符串中
`所有的字符串必须使用双引号`
一个完整的JSON字符串，前后的括号必须对应，且不能有其他内容

JSON字符串与JS数据转换

- JS -> JSON(序列化)

```javascript
var jsonStr = JSON.stringify(JS数据);
```

- JSON -> JS (反序列化)

```javascript
var JS数据 = JSON.parse(JSON字符串);
```


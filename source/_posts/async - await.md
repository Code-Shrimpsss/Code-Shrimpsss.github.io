---
title: anync / await
date: 2021-10-05 00:00:00
tag: 前端技术
description: 'anync / await 与微任务宏任务'
cover: https://s3.bmp.ovh/imgs/2021/12/e57b8469ed115292.png
---

# anync / await 原理 #

------

## 知识点 ##

- await只能在async函数中使用，不然会报错
- async函数返回的是一个状态为fuifilled的Promise对象，有无值看有无return值
- await后面只有接了Promise才能实现排队效果
- async/await作用是**用同步方式，执行异步操作**

## 注意 ##

- 在 async 方法中， 第一个 await 之前的代码会同步执行， await 之后的代码会异步执行





# EventLoop #

------

为了防止某个 **耗时任务** 导致 **程序假死** 的问题 ，JavaScript 把待执行的任务分为两类：

- 同步任务(  synchronous )

  又叫非耗时任务 ， 指的是在主线程上排队执行的那些任务

  **只有前一个任务执行完毕，才能执行后一个任务**

- 异步任务 ( asychronous )

  又叫做耗时任务，异步任务由JavaScript 委托给宿主环境进行执行

  **当异步任务执行完成后，会通知JavaScript主线程执行异步任务的回调函数**

  ![Snipaste_2021-09-27_18-08-47.png](https://i.loli.net/2021/09/27/WwmuyF8K2YHa3JO.png)

  

  **JavaScript 主线程从 “任务队列” 中读取异步任务的回调函数，放到执行栈中依次执行。**

  这个过程是循环不断的，所以整个的这种运行机制又称为 **EventLoop**

# 宏任务与微任务 #

------

```javascript
// 循环机制
` 宏任务 --> 渲染 --> 宏任务 --> 渲染 --> 渲染．`
```

https://juejin.cn/post/6994265017595985956

当 宏任务执行完，会在渲染前，将执行期间所产生的所有 微任务都执行完**

**在当前的微任务没有执行完成时，是不会执行下一个宏任务的。**

### 宏任务 ###

|           #           | 浏览器 |
| :-------------------: | ------ |
|          I/O          | ✅      |
|      setTimeOut       | ✅      |
|      setInterval      | ✅      |
|     setImmediate      |        |
| requestAnimationFrame | ✅      |

### 微任务 ###

|              #               | 浏览器 |
| :--------------------------: | ------ |
|       process.nextTick       | ❌      |
|       MutationObserver       | ✅      |
| Promise.then  catch  finally | ✅      |


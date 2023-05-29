---
title: Promise简述
date: 2020-11-05 00:00:00
description: 'Promise深入浅出'
tag: 前端技术
cover: https://s3.bmp.ovh/imgs/2021/12/08de4704b3397070.png
---

<!-- <div id="posts-calendar"></div> -->

# **Promise** #

------

### **Promise是JS中进行异步编程的新的解决方案** ###

- promise： 是一个**构造函数**
- Promise的构造函数接收一个参数，是函数，并且传入两个参数：**resolve，reject**，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。
- new 出来的 Promise 实例对象，代表一个**异步操作**
- 每一次 new Promise() 构造函数得到的实例对象，都可以**通过原型链**的方法访问到.then方法
- .then() 方法用来**预先指定成功和失败的回调函数**
- 调用 .then() 方法时，**成功的回调函数是必选的**，失败的回调函数是可选的
- catch() 方法用来**捕获与处理错误**

## Promise 的状态改变 ##

pending  -->  resolved

pending -->  rejected 

说明: 只有这 2 种, 且一个 promise 对象只能改变一次 

**无论变为成功还是失败, 都会有一个结果数据** 

成功的结果数据一般称为 vlaue, 失败的结果数据一般称为 reason

## 流程图 ##

![Snipaste_2021-09-27_14-59-26.png](https://i.loli.net/2021/09/27/RAEBhIFUeSvQnm3.png)

## 为什么要用Promise ##

1. 指定回调函数的方法更加灵活
2. 支持链式调用 ，可以解决回调函数问题

## 基础 ##

```javascript
1. Promise 构造函数: Promise (excutor) {} 
excutor 函数: 同步执行 (resolve, reject) => {} 
resolve 函数: 内部定义成功时我们调用的函数 value => {} 
reject 函数: 内部定义失败时我们调用的函数 reason => {} 
说明: excutor 会在 Promise 内部立即同步回调,异步操作在执行器中执行 

2. Promise.prototype.then 方法: (onResolved, onRejected) => {} 
onResolved 函数: 成功的回调函数 (value) => {} 
onRejected 函数: 失败的回调函数 (reason) => {} 
说明: 指定用于得到成功 value 的成功回调和用于得到失败 reason 的失败回调 返回一个新的 promise 对象 

3. Promise.prototype.catch 方法: (onRejected) => {} 
onRejected 函数: 失败的回调函数 (reason) => {} 
说明: then()的语法糖, 相当于: then(undefined, onRejected) 

4. Promise.resolve 方法: (value) => {} 
value: 成功的数据或 promise 对象 说
明: 返回一个成功/失败的 promise 对象 

5. Promise.reject 方法: (reason) => {} 
reason: 失败的原因 
说明: 返回一个失败的 promise 对象 

6. Promise.all 方法: (promises) => {} 
promises: 包含 n 个 promise 的数组 
说明: 返回一个新的 promise, 只有所有的 promise 都成功才成功, 只要有一 个失败了就直接失败 

7. Promise.race 方法: (promises) => {} 
promises: 包含 n 个 promise 的数组 
说明: 返回一个新的 promise, 第一个完成的 promise 的结果状态就是最终的 结果状态
```

## Promise关键问题 ##

1. **如何改变 promise 的状态?** 

(1) resolve(value): 如果当前是 pendding 就会变为 resolved 

(2) reject(reason): 如果当前是 pendding 就会变为 rejected 

(3) 抛出异常: 如果当前是 pendding 就会变为 rejected 

2. **一个 promise 指定多个成功/失败回调函数, 都会调用吗?** 

当 promise 改变为对应状态时都会调用 

3. **改变 promise 状态和指定回调函数谁先谁后?** 

(1) 都有可能, 正常情况下是先指定回调再改变状态, 但也可以先改状态再指定回调 

(2) 如何先改状态再指定回调? 

1   在执行器中直接调用 resolve()/reject() 

2   延迟更长时间才调用 then() 

(3) 什么时候才能得到数据? 

1   如果先指定的回调, 那当状态发生改变时, 回调函数就会调用, 得到数据 

2   如果先改变的状态, 那当指定回调时, 回调函数就会调用, 得到数据 

4. **promise.then()返回的新 promise 的结果状态由什么决定?** 

(1) 简单表达: 由 then()指定的回调函数执行的结果决定

(2) 详细表达: 

1  如果抛出异常, 新 promise 变为 rejected, reason 为抛出的异常 

2  如果返回的是非 promise 的任意值, 新 promise 变为 resolved, value 为返回的值 

3  如果返回的是另一个新 promise, 此 promise 的结果就会成为新 promise 的结果 

5. **promise 如何串连多个操作任务?** 

(1) promise 的 then()返回一个新的 promise, 可以开成 then()的链式调用 

(2) 通过 then 的链式调用串连多个同步/异步任务 

6. **promise 异常传透?** 

(1) 当使用 promise 的 then 链式调用时, 可以在最后指定失败的回调, 

(2) 前面任何操作出了异常, 都会传到最后失败的回调中处理 

7. **中断 promise 链?** 

(1) 当使用 promise 的 then 链式调用时, 在中间中断, 不再调用后面的回调函数 

(2) 办法: 在回调函数中返回一个 pendding 状态的 promise 对象 


---
title: 'JacaScript解刨'
date: 2020-03-22 00:00:00
description: '对JS的笔记一些个人理解与实用知识'
tag: JavaScript
cover: https://s3.bmp.ovh/imgs/2021/12/4d4b264f31082964.png
---

# JavaScript  -  高级 #

## this ##

![1616466039075](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1616466039075.png)

==call==

call()方法调用一个对象。可立即调用函数与改变this指向；

```javascript

```

==apply==

apply()方法调用一个函数，可以立即调用函数与改变this指向；

```javascript

```

==bind==

bind()方法不会调用函数，但可以改变函数内部this指向，返回的是原函数改变this之后产生的新函数。

- 共同点：都可以改变this指向；

- 不同点：call 和 apply 会调用函数，并且改变函数内部this指向；

- call传递参数使用逗号，apply使用数组传递；

- bind 不会调用函数，可以改变函数内部this指向；

```javascript

```

  —— 应用场景

-   call 经常做继承；
- apply 经常跟数组有关系；比如借助于数学实现数组最大值与最大值
- bind 不调用函数，可以改变this指向；比如改变定时器内部的this指向

##  严格模式 ##

1 · 为脚本开启严格模式

```javascript
1，(function(){
    //当前这个自调函数中开启严格模式，函数之外还是普通模式
    "use strict"
   }
   )()
2, <script>
    //当前的script标签开启了严格模式
   "use strict 
   </script>
```



## 高阶函数 ##

map() : 接收一个函数参数的数组对象，遍历这个数组；

reduce() : 接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值；

filter() : 接收一个函数参数的数组对象，返回满足条件的数组对象；

forEach() : 调用数组中的每个元素，并将元素传递給回调函数；

some() : 判断是否含有符合条件的元素，返回布尔值；

every() : 判断是否全部元素符合条件，返回布尔值；

## 对象原型链 ##

- ### prototype ###

每个函数都有prototype原型属性；这是一个对象所有函数的属性和方法都被构造函数所拥有

==好处：==可以把不变的方法直接定义在prototype对象上，直下的所有对象实例都可以 （共享）这些方法。

```javascript
Star.prototype.sing = function(){
    console.log('唱歌');
}
var BOB = new Star("");
BOB.sing();//唱歌
```

- ### 对象原型 ###

对象都会有一个属性__ prpto __ 指向构造函数的prototype原型对象； __ porto__ 对象原型和原型对象prototype是等价的；

```javascript
Star.__proto__sing = function(){}
```

- ### constructor构造函数 ###

对象原型（__ proto __）和构造函数（prototype）原型对象里面都有一个属性constructor属性的构造函数，它指回构造函数本身；

它可以让原型对象重新指向原来的构造函数。

```javaScript
//如果修改了原来的原型对象，给原型对象赋值的是一个对象，则必须手动利用constructor指回原 来的构造函数
 constructor : OBJ,
```

![1616746783081](C:\Users\97830\AppData\Roaming\Typora\typora-user-images\1616746783081.png)

## 闭包 ##

1. 闭包（closure）指有权访问另一个函数作用域中变量的函数；
2. 一个作用域可以访问另一个函数的局部变量
3. 我们fn 外面得作用域可以访问fn 内部的局部变量
4. 闭包的主要作用：延伸了变量的作用范围

## 递归 ##

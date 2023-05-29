---
title: 你不知道的Array.from
date: 2021-06-09 00:00:00
type:
description: '原来Array.from还有这种操作'
tag: JS-ES6
cover: https://s3.bmp.ovh/imgs/2021/12/08de4704b3397070.png
---

# Array.from

可参考 MDN 里的 Array.from

> https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from

```javascript
var obj = {  0:1, 1:2, 2:3,
           length:3, // 1
           length:4, // 2
           length:0, // 3
           push:[].push //可在对象中定义数组方法
          };
obj -> 伪类 ->类似于数组的一种伪类型 -> 不是JS类型
var newArr = Array.from(obj);
console.log(newArr);
// 1: [1,2,3]; 2:[1,2,3,unfeined]; 3: []
//length决定了Array.from最终返回的新数组的长度，剪裁掉或补齐 (undiefined)
console.log(newArr.length);
obj.push(4);
console.log(obj);// [1,2,3,4]
```

## Funny reference val

```javascript
`function Array Object -> 引用值 ->Object.prototype`;
var obj1 = {};
obj1.a = 1;
var newObj = new Object(obj1); //  newObj = obj1;
console.log(newObj === obj1); //true | {a:1} - {a:1}

var arr1 = [1, 2, 3, 4];
console.log(new Object(arr1)); // [1,2,3,4]

var arr = [];
arr.a = 1;
var newarr = new Array(arr);
console.log(newarr == arr); //false | [a:1] - [Array(0)] : 0-> [a:1]

var str = '123'; // 原始值
console.log(str.__proto__); //原始类型

var a = 1; // 原始值
var newA = new Number(a); //引用值
console.log(a == newA); //true | 隐式类型转换
console.log(a === newA); //false | 1 - Number{1}
```

## Object

```javascript
        var obj ={
            a:1,
            b:2,
            c:3
        }
        Object.defineProperty(obj,'b'{
            enumerable:false
        })
        // 应用场景不一样
        console.log(Object.keys(obj));
        // 忽略描述符 -enumerable:false
        console.log(Object.getOwnPropertyNames(obj));
```

---
title: 你不知道的undefined
date: 2020-10-05 00:00:00
tag: 前端技术
description: 'undefined原来是这样'
cover: https://s3.bmp.ovh/imgs/2021/12/e57b8469ed115292.png
---

# 前言

本文讲述了 JavaScript 中 `undefined`的规范定义与一些场景下的小知识，以及 `void()`与的它的区别

# 什么是 undefined

`undefined` 既是一个原始数据类型，也是一个原始值数据

`undefined`  属性表示变量没有被赋值，或者根本没有被声明

`undefined` 是全局对象上的一个属性 **window.undefined**

# undefined 四大不

1. `不可写  writable:false`

```javascript
window.undefined == 1;
console.log(window.undefined); // undefined
```

2. `不可配置  configurable:false;`

```javascript
delete window.undefined;
console.log(window.undefined); // undefined
```

3. `不可枚举 enumerable:false`

```javascript
for (var k in window) {
	if (k === undefined) {
		console.log(k); //undefined
	}
}
```

4. `不可重新定义`

```javascript
Object.defineProperty(window, 'undefined', {
	enumerable: true,
	writable: true,
	configurable: true,
	// Cannot redefine property:undefined
});
```

# 作用域下的 undefined

虽然全局作用域下的 `undefined` 是不可写，打印出来还是 undefined，但是局部作用域下的 `undefined` 相对于一个变量，它在局部作用域下**不被识别为 JavaScript 中的保留字与关键字**

```javascript
`全局`;
window.undefined == 1;
console.log(window.undefined); // undefined

`作用域`;
function test() {
	var undefined = 1;
	console.log(undefined);
}
test(); // 1

'use strict'`严格模式下 函数内外也是如此`;
```

# 未定义的 undefined (小知识)

### **funny one**

```javascript
function test(a) {
	console.log(typeof a); //undefined
	return a; //undefined
}
console.log(test());
```

`log1:` 形参没有传递相应的值 就为 undefined

`log2:` 类型返回值 同为 undefined

### **funny two**

```javascript
var a = null;
console.log(a == undefined); //true
console.log(a === undefined); //false
```

`log1:` null == undefined，因为 undefined 值是由 null 值派生而来的

`log2:` null !=== undefined， 因为严格判断 undefined 只等于本身

### **funny three**

```js
console.log(b); // b is not defined
console.log(typeof b); // undefined
```

`log1:` b 未声明 报错

`log2:` 因为未声明未赋值 b 类型为 undefined

### **funny four**

```js
document.all == undefined; //true
```

`log1:` 根据文档说明 document.all()可以查阅 IE 浏览器版本； 不为 IE 浏览器的其他浏览器，类型均为 undefined。 ƪ(˘⌣˘)ʃ

### **funny five**

```js
var a;
if ('a' in window) {
	console.log(true);
} else {
	console.log(false);
} // true
```

`log1:` 虽然 a 的类型为 undefined，但 a 并不在 window 中

# void (expression)

**void()不管输入什么 始终返回 undefined**

ps：void 不会像 undefined 一样被局部作用域而改动，类型与返回值始终为 undefined

### **funny one**

```javascript
var a, b, c;
a = void ((b = 1), (c = 2));
console.log(a, b, c);
//log1:
```

`log1:` undefined 1 2；

### **funny two**

```javascript
<a href="JavaScript:void(0)"> or <a href="JavaScript:；">
```

`log1:` return: undefined

`log2:` 这种写法称为阻止 伪协议；也叫无效链

### **funny three**

```javascript
console.log(void 0 === window.undefined); // true
```

`log1: ` void(0) 等同与 undefined

### **funny four（重点）**

```javascript
function test() {
	var undefined = 1;
	console.log(undefined); // 1
	console.log(void 0 === void 999); // undefined
	console.log(undefined === void 0); // false
}
test();
```

`log1:` undefined 被声明为局部变量

`log2:` 不管 void() 数值多少 始终等于 undefined

`log3:` undefined 已为局部变量，值并不为 undefined

# 总结

-   undefined 的禁忌规范 **不可写**，**不可配置**，**不可枚举**，**不可重新定义**
-   全局作用域下与局部作用域的 `undefined`
-   void 的好处就是在需要定义 undefined 时候可**灵活性**比 undefined 本身要好
-   用`void(0)`来代替`undefined`进行赋值**更安全**

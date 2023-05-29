---
title: JS - Math.random()
date: 2021-02-24 00:00:00
tag: JavaScript
description: 'JS - Math.random() 随机数'
cover: https://s3.bmp.ovh/imgs/2021/12/4d4b264f31082964.png
---

# JS - Math.random() 随机数 #

### 	1. Math.random()  ： 随机获取范围内的一个数 （ 精确到小数点后14位 ） ###

#### 生成0~60间的数 ####

```javascript
// 语法: Math.random()\*m [表示生成0~m之间的随机数]
Math.random() * 60 
```

#### 生成0~60间的整数 ####

```javascript
parseInt(Math.random()*60)
```

#### 向下取整整数 ####

```javascript
Math.floor(Math.random()*(60+1))
```

#### 生成1~60间的整数 ####

```javascript
Math.floor(Math.random()*60)+1
```

### 	2. 表示生成两个数之间的随机数 ###

```javascript
// 语法: Math.random()*m+n [表示生成n~m+n之间的随机数]
console.log(Math.random()*10+8); // 8~18 
```

```javascript
// 语法: Math.random()*m-n [表示生成-n~m+n之间的随机数]
console.log(Math.random()*10-8); // -8~2
```

```javascript
// 语法: Math.random()*m-m [表示生成-m~0之间的随机数]
console.log(Math.random()*10-10); //-10-0
```

### ​	3. 生成n~m之间的随机整数（包括n与m） ###

```javascript
// 语法: Math.floor(Math.random()\*(m-n+1))+n
console.log(Math.floor(Math.random()*(8-100+1))+100);  *// 8~100*
```
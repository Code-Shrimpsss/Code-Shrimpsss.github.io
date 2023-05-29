---
title: Flow.js
date: 2021-7-28 00:00:00
type:
description: 'Flow.js入门'
cover: https://s3.bmp.ovh/imgs/2021/12/4d4b264f31082964.png
---


# Flow #

------

参考文献：https://www.cnblogs.com/zhuzhenwei918/p/7150762.html

最全的手册：https://www.saltycrane.com/cheat-sheets/flow-type/latest/#builtins

Flow.js是FaceBook发布的开源Javascript静态类型检查器。他给JavaScript提供了静态类型来提高开发人员的生产力和代码质量。

```javascript
//查看全局安装的npm包
npm list -g --depth 0
```

#### 初始化 ####


```typescript
// 第一步 安装初始化package.json文件
yarn init --yes
// 第二步 安装flow依赖包
yarn add flow-bin --dev
// 第三步 创建flow配置文件
yarn flow init
// 第四步 启动flow服务
yarn flow start
// 第五步 服务启动后，后续即可使用flow命令检查项目文件
yarn flow
// 停止flow服务
yarn flow stop
// 直接用flow check命令可以不启动服务做文件检查。
yarn flow check
```



### 类型标注 ###

```javascript
// @flow

function square(n: number): number {
   //意为参数与返回值都必须为number类型
return n * n;
}
square("2"); // Error!
```

### 完成编码过后自动移除类型注解 ###

##### 方法一 #####

```javascript
// 安装模块
yarn add flow-remove-types --dev
// 使用模块的命令行工具
yarn flow-remove-types . -d dist // .(当前目录[一般用src]) -d(转换) dist(指定目录) 
```

##### 方法二 #####

```JavaScript
// 1.安装命令
yarn add @babel/core @babel/cli @babel/preset-flow -dev
// @bable/core = bable的核心模块
// @babel/cli = babel工具，可以在命令行中直接使用命令完成编译
// @babel/preset-flow = 包含了转换flow类型注解的插件

// 2.项目根目录新建 <.babelrc> 文件
// 输入：
{
    "presets":["@babel/preset-flow"]
}

// 3. 转换命令
yarn babel src -d dist 
// 意为把src中的所有文件转换到dist文件当中
```

### 语法 ###

#### `基础变量` ####

```typescript
const a: string = 'Hey'
const b: number = 100 // NAN // Infinity
const c: boolean = true // false
const d: null = null
const e: void = undefined
const f: symbol = Symbol()
```

#### `数组类型限制` ####

```javascript
// 1.表示创建一个每一项值只能为数字的数组
const arr: Array<number> = [1,2,3]
// or
const arr1: number[] = [1,2,3]

// 2. 表示固定长度的指定类型数组
const foo: [string,number] = [1,2,3]
```

### `对象类型限制` ###

```javascript
// 创建一个对象变量 里面存放对应的键值类型
const obj1: { foo: string, bar: number } = { foo: 'string', bar: 100 }

// 在键后加 ？ 为可选可不选
const obj2: { foo?: string, bar: number } = { bar: 100 }

// 可新建变量，后续再存放键值
const obj3 = {}
obj3.key1 = 'value1'

// 新建变量规定键值后再存放
const obj3： { [number]: number} = {}
obj3.key1 = 100
```

#### `函数类型限制` ####

```javascript
// 指定实参必须为数字类型
function sum (a:number,b:number){
    return a*b
}

// ：number定义函数返回值只能为number类型
function foo():number{  
   return (typeof number)
}

 // ：void定义函数返回值只能为undefined类型
function foo(): void{  
   return (typeof undefined)
}

// 设置回调函数限制
// function foo(callback:(类型) => 返回值)
// 表示必须填入两个类型相对应的实参，返回值为undefined(不可有返回值)
function foo(callback:(string,number) => void){
    // 回调函数所传入的实参
    callback('who am i', 42)
}
// 调用回调函数
foo(function(str,n){
    // str => string
    // n = number
    `不可有返回值`
})
```

#### `特殊类型` ####

```javascript
`字面量类型`

// 普通字面量，必须一一对应
const a: 'Hey' = 'Hey' 
// 联合(或)类型 
// 选其一(值)
const b: 'Hey' | 'Hello' | 'Hi' = 'Hello'
// 选其一(类型)
const c: string | number = 'string'
const d: StringOrNumber = string | number

`meybe`
// 问号为特定给予的undefined与null
const gender: ?number = undefined // null // number 
// 等同于
const gender: number | void | null = undefined 

`mixed与any`
// 意为所有类型
function passMixed(value: mixed | any){
    passMixde('可以是所有类型如：string number')
}

// 区别:mixed为强类型 ，any为弱类型

// --- any
function passAny(value: mixed){
    // 可以直接使用如字符串的substr方法
    value.substr(1)
    // 也可以做算法运算
    value. * value
}
PassAny('string')；
PassAny(100)；

// --- mixed
function passMixed(value: mixed){
    // 不可直接运算及使用方法
    // value. * value => X 在编译时报错
    
    //必须先明确标注类型，才可使用对应方法`
    if(typeof value === 'string' ){
        value.substr(1)
    }
}
PassMixed('string')


// 建议使用Mixed进行编译
```

#### 环境API ####

```typescript
如： const element：HTMLElement | null = document.getElementById('app')
// HTMlElement为flow下载时的临时文件（包）

// 环境API对应的声明文件
https://github.com/facebook/flow/blob/main/lib/core.js
https://github.com/facebook/flow/blob/main/lib/dom.js
https://github.com/facebook/flow/blob/main/lib/bom.js
https://github.com/facebook/flow/blob/main/lib/cssorm.js
https://github.com/facebook/flow/blob/main/lib/node.js
```


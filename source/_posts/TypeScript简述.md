---
title: TypeScript简述
date: 2018-01-05 00:00:00
type:
description: 'TypeScript入门指南'
cover: https://s3.bmp.ovh/imgs/2021/12/ef8145bf1a911fb1.png
---



# TypeScript #

------
- TypeScript 是`静态类型`

    静态类型是指编译阶段就能确定每个变量的类型
    这种语言的类型错误往往会导致语法错误。TypeScript 在运行前需要先 为 JavaScript，而在编译阶段就会进行类型检查

- 强类型：语言层面限制函数的实参类型必须与形参类型相同

    注 ：`强类型`中不予许随意的数据隐式类型装换，而`弱类型`允许；

    <!-- 动态类型：允许随时修改变量的类型 -->

## 初始化 ##

### 安装 ###

```javascript
// 1.使用yarn配置package.json
yarn init --yes
// 2.安装typescript开放依赖
yarn add typescript --dev
`这时node_modules/bin目录中会有tsc文件, 用来编译typescript文件`
```

### 配置 ###

```typescript
// 添加tsconfig.json配置
yarn tsc --init

//  * 提前配置
"sourceMap": true,   // 存储源代码与编译代码对应位置映射
"outDir": "dist",    // 编译后存放的目录
"rootDir": "src",    // 从这个文件编译
```

### 常用语法 ###

```typescript
// 快速编译（编译后放在同目录）
yarn tsc test.ts(需要编译的ts文件)
// 快速编译（ * 提前配置 - 从指定目录编译到指定目录）
yarn tsc
// 显示中文
yarn tsc --locale zh-CN
```

### 作用域问题 ###

因为ts的变量设计是面向全局的，所以相同文件目录下的ts文件变量名称相等会照成冲突
**解决办法**
```typescript
// 创建立即执行函数
(function(){
    const a = 123
})()

// 以一个包的形式导出
export {}
```

## 基础数据类型 ##
**JS的八种内置类型**

变量的类型由后面声明的类型决定
```typescript
let str: string = "Shrimps";
let num: number = 22;
let bool: boolean = true;
let u: undefined = undefined;
let n: null = null;
let obj: object = {x: 1,y: 2};
let big: bigint = 100n;
let sym: symbol = Symbol("hey");
```


## 类型限制 ##

### 对象 ###

```typescript
// 创建一个对象变量
const foo: object = function() {} //[] //{}

// 创建一个对象变量 里面存放对应的键值类型
const obj: { foo: number, bar:string } = { foo: 123, bar: 'Hey'}
```

### 数组 ###

```typescript
`两种创建方式`
// 创建一个数组 （只能存放string类型）
const arr1: Array<string> = ['七','里','香']
// or 两种创建方式
const arr2: number[] = [12,54,123,342,1]


// 高效定义一个传递返回的数组 (减少类型判断)
function sum(...args: number[]){
    return args.reduce( prev, current ) => prev * current, 0)
}
sum(2,33,12,41,91)
```

### 元组 ###

`列表是可以修改的数据结构，而元组是固定长度，不能被修改元素值的数据结构。`

```typescript
// 创建一个元组 键值类型必须与值对应
const str: [number,string] = [22,'KOS']

// 也可以解构提取数值
const [ age,name ] = str

Object.entries({
    foo:123,
    bar:'sad'
})

```

### 枚举 ###

`枚举enum是一种特殊的类(但枚举是类)，使用枚举可以很方便的定义常量`

```typescript
// 模拟枚举
enum PostStatus  {
    Draft = 1,
    Unpublished = 1,
    Published = 2
}
```

这种通过等号的显式赋值称为 `initializer`。如果枚举中某个成员的值使用显式方式赋值，但后续成员未显示赋值， TypeScript 会基于当前成员的值加 1 作为后续成员的值

**这种方式编译后对应文件会显示编译代码**

```typescript
// 建议使用常量枚举， 就不会显示编译代码 如：
const enum PostStatus {
   // 可以省略等号，会自动从头开始按照顺序排序
   Draft,
   Unpublished,  // 2
   Published = 8,
   testStr, // 9
}
```

### 函数 ###

```typescript
// 函数类型显示
function func1 (a: number,b?: numebr = 10 ): string{
            // ( ...rest:number[] )  -- 选择传入任意个数的参数
            // b?: number  -- 为number类型b参数可选可不选
            // = 10  -- 默认参数值, b 可以不用传参数了 
            // : string --- 返回值必须为string类型
    return 'Who am i'
}
func1(100,33) // 形实对应类型一致
```

### 任意 ###

any可以接收任意类型参数

```typescript
function stringify(value: any){
    return JSON.stringify(value)
}
// 因为有可能会存放任意类型的值，所以ts不会对这个类型做检查
// 在语法上不会报错 存在安全隐患 不建议用
stringify('string')
stringify(true)
stringify(100)

// * Unknow 类型和 any 一样可以容纳任意类型比 any 安全
```

### 类型别名 ###

使用 type 给类型取别名

```typescript
type test = number
let counts ：test = 124

type tit = string | boolean
let str ：tit = 'hey'
let str1 : tit = true
```

### 交叉类型 ###

用 `&` 进行连接

```typescript
// 把类型都组合起来，变量赋值必须满足 交叉类型
interface Itest {
    key: string
}

type test = Itest & { value: string }
let mytest: test = { key：'No.1'，value：'苦瓜柠檬' }
```

### 隐式类型推断 ###

```typescript
//因为typescript是静态类型 不可轻易改值
let age = 18
age = 'string' // X 报错

// 可预定义变量 后再添类型
let test 
test = false // typeoof boolean

// 跟any同个性质 也会造成安全隐患
// 建议在ts编译时给每一个变量都要加上类型
```

### 类型断言 ###

```typescript
// 明确指定类型 - 两种写法
const num1 = res as number // 建议
// or
const num2 = <number>res // JSX 下不能使用

// 类型断言 并不是 类型转换
// 类型转换是在代码编译后的概念 
// 类型断言是在代码编译中的概念，编译后这个断言就不会存在了
```



## 接口 ##

```typescript
// 接口 interface : 将有结构的数据约束的结构

// 定义接口
interface Post {
    title: string
    content: string
}
// 使用接口参数
function printPost(post: Post){
    console.log(post.title);
    console.log(post.content);
}
// 使用实参
printPost({
    title: 'Hello World',
    content: 'Welcome my faveion firend'
})
```

### 接口成员 ###

```typescript
`可选成员与只读成员`
interface Post {
    title: string
    // 可选成员 
    subtitle?: srtring  //  ? 在这里的意思是 string | undefined 
    // 只读成员
    readonly summary: string // 在初始化完成后，就不能进行修改了

}

`动态成员`
//新建一个接口
interface Cache {
    // 储存一个合集 键与值都要为string
    [prop: string]: string
}
// 引用Cache的规则
const cache: Cache = {}
// 键值都得为string
cache.foo = 'i am string'
cache.bar = '我是字符串鸭'
```



## 类 ##

### 基本使用 ###

- **父类**称为（超类），**子类**被称为（派生类）
- 派生类如果包含一个构造函数constructor，则必须在构造函数中调用 super() 方法 !

```typescript
class Person {
    // 声明变量 与 属性的初始值
    name: string
    age: number
    // 从以声明的变量中使用
    constructor(name: string,age: number) {
        this.name = name
        this.age = age
    }
}
// 函数声明变量
sayHi(msg: string): void {
    console.log(`I am ${this.name},${msg}`);
}
```

### 类的访问修饰符 ###

```typescript
class OneLine {
  // 公用成员
  public name: string;
  // 私有成员
  private age: number;
  // 收保护的
  protected gender: boolean;

  constructor(name:string , age:number){
  this.name = name,
  this.age = age,
  this.gender = true
  }
}

const oneli = new OneLine("Shrimps", 20);
console.log(oneli.name); // 可以使用
console.log(oneli.age); //  error
console.log(oneli.gender); //  error
// 因为age定义private 与 gender定义protected 所以不能在外界引用

// 区别在于 protected 可以在类的子类中使用 如：
class TwoLine extends OneLine {
    constructor(name: string , age: number){
        super(name, age)
        console.log(this.age);    //private // error
        console.log(this.gender); // protected

    }
}
```

![](https://i.loli.net/2021/08/26/FdhGsgnDouatAV8.png)

```typescript
// 构造函数 constructor 的事件修饰符默认是public
// 也可以变换 private | protected 规则方法大同小异
provate constructor(...){ ... } 

`只读属性 readonly `
class Test {
    // 设置为只读的属性，实例只能读取这个属性值，但不能修改
     readonly test: void
    // 如果只读修饰符和可见性修饰符同时出现，需要将只读修饰符写在可见修饰符后面
     protected readonly test: void
}
```

### 类类型接口 ###

- 使用接口可以强制一个类的定义必须包含某些内容
-  要使用接口，需要使用关键字`implements`
- **implements** 关键字用来指定一个类要继承的接口，如果是接口和接口、类和类直接的继承，使用`extends`，如果是类继承接口，则用`implements`。

```typescript
// 要求使用该接口的值必须有一个对应属性 如：eat
interface Eat {
    eat (food: string): void
}
// 建议单独接口写单独规则 不会冲突
interface Run {
    run (run: string): void
}

// 通过 implement 继承 对应两个接口
class Person implements Eat,Run{
    eat (food: string): void{
    console.log('人类的吃法');
    }
    run (run: string): void{
    console.log('直立行走');
    }
}

class animal implements Eat,Run {
    eat (eat: string): void {
        console.log('动物的吃法');
    }
    run (run: string): void{
        console.log('爬行');
    }
}
```

### 抽象类 ###

- 抽象类一般用来被其他类继承，而不直接用它创建实例。

- 抽象类和类内部定义抽象方法，使用`abstract`关键字：

```typescript
abstract class Animals {
    eat (eat: string): void {
        console.log('动物们的吃法');
    }
    // 定义抽象方法 // * 不需要方法体
    abstract run (distance: number): void
}

class Dog extends Animals {
    // 可在子类中添加方法
    run (distance: number): void{
        console.log('四脚爬行', distance);
    }
}
const dogs = new Dog()
// 同时拥有父类及自身的实现方法
dogs.eat('来福')
dogs.run(999)

// * 抽象方法和抽象存取器都不能包含实际的代码块。
```

### 泛型 ###

`泛型`就像一个占位符一个变量，在使用的时候我们可以将定义好的类型像参数一样传入，原封不动的输出

```typescript
// 一般类型
function createNumberArray (length: number, value: number): number[] {
    const arr = Array<number>(length).fill(value)
    return arr
}

const res = createNumberArray(3,77)
// res =>  77 77 77

// 泛型就是把我们定义时不能够明确的类型变成参数
// 类型参数一般用 <T> 定义 
function createArray<T> (length: number, value: T): T[] {
    const arr = Array<number>(length).fill(value)
    return arr
}
// 需使用时再去传递这个类型参数
const res = createArray<string>(3,'hey')
// res =>  hey hey hey
```


<!-- > 参考文档 [Typescript-2w字总结]() -->
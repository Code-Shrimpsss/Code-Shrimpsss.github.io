---
title: JavaScript 总结
date: 2020-02-24 00:00:00
description: '从浅薄到深厚对JS的一些总结'
tag: JavaScript
cover: https://s3.bmp.ovh/imgs/2021/12/4d4b264f31082964.png
---

# JavaScript 总结

#### JavaScript组成

$$
1，ECMAScript 核心语法
2，DOM 文档对象模型
3，BOM 浏览器对象模型
$$

------

## 变量

```js
概念：容器，用来存储数据
语法: var 变量名 = 数据；

变量名命名规则：
1. a-zA-Z,_ $ 数字0-9 组成
2. 不能以数字开头
3. 不能以关键字(var,for if ),保留字(class)作为变量名
4. 命名有意义
5. 对于变量  user_name  由于函数  getName(了解) 驼峰命名
```



## 数据类型

```javascript
1，基本数据类型：
Number（数字型），Boolean（布尔），String（字符串），Undefined（未定义），null（空）；

2，复杂数据类型：
Object（对象）  Array（数组） function（函数）；

数据类型判断：
typeof 数据 ； typeof (数据)
typeof null -> object
```



## 运算符

```javascript
1.算数运算符
+ - * / %

隐式装换：
·1，确定数据类型
·2，算数运算符的类型
+ 1，number + string -> 拼接  （number 转 string） 
  2，number + boolean -> 加运算 （boolean 转 number）
- 1，number - string -> 减运算 （string 转 number）
  2，number - boolean -> 减运算 （boolean 转 number）
2.逻辑运算符
·1，！： 非
·2，&& ： 与
·3，|| ：或
3.赋值运算符
· +=  
· -=  
· *= 
· /=
· %=
4.比较运算符
· > （大于）
· < （小于）
· >=（大于并等于）
· <=（小于并等于）
· =  (赋值)
· ==（判断数据是否相同）
· ===（全等判断（ 1，数据类型相同 2，数据内容相同））
· !=（不等）
· !==（不等（严格判断））
5. 自增自减
· 前置++
· 后置++
· 前置--
· 后置--
```



## 数据类型装换

```javascript
转数字
1,Number '100' => 100 但是'100a' -> NaN
2,parseInt '100a' -> 但是 '100.97a' -> 100
3,parseFloat '100.97a' -> 100.97
4,+ * / %  +'100' -> 100 
注意：只要以非数字开头，结果都是NaN
```



## 条件表达式

```javascript
1，if 语句
语法： if(条件表达式){
    语句块，只有条件成立，才会执行语句块
}else{}
2，if else if多条件分支
if(表达式1){语句1}
else if(表达式2){语句2}
else if(表达式3){语句3}
....
else{}
3,switch 语句
switch(表达式){
    case 值 1:
       语句 1:
       break:
    case 值 2:
       语句 2:
       break:
    case 值 3:
       语句 1:
       break:
       ....
       default:
        默认语句;
        break;
}
4,三元表达式

表达1 ？ 语句1 : 语句2 ；
```



# 循环

```javascript
1， for 循环
语法： for(初始化变量;条件表达式;操作表达式){
    //循环体
}
1.2 双重 for 循环
语法： for(外循环的初始;外循环的条件;外循环的操作表达式){
    for(内循环的初始;内循环的条件;内循环的操作表达式){
        //需执行的代码
    }
}
2， while 循环
(循环体代码执行完毕后，程序会继续判断执行条件表达式，如条件仍为true，则会继续执行循环体，直到循环条件为 false 时，整个循环过程才会结束)
语法： while(条件表达式){
      //循环体代码
      }
3， do while 循环(先执行一次循环体代码)
do{
    //循环体代码 - 条件表达式为true 时重复执行循环体代码
}while(条件表达式);
· 跳出循环
continue:用于立即跳出本次循环，继续下一次循环
break:用于立即跳出整个循环(循环结束)
```



## 数组

```javascript
1，作用：保存多份任意数据

2，语法：
  1，字面量创建数组
  var arr = [数据1，数据2，数据3，...数据n]// arr数组名字
  2，构造函数创建数组
  var arr = new Array()(了解)
注意： 1， new Array(10) //创建一个长度为10的空数组
       2， new Array(10,20,30)//创建一个长度为3的数组

3，数组长度：  数组名.length

4，下标/索引
   var arr = [1,2,3,4,5]数组下标从0开始
   
5，获取数组中数据
  1，数组名[下标]
  2，数组最后一项:[数组名.length - 1]//从下标0开始所以减一

6，新增数据
1， var arr = [1,2,3,4,5,6] -> arr[5] = 数据
数组名[下标] = 数据    数组名[数组名.length] = 数据

7，修改数据
var arr = [1,2,3,4,5]  arr[下标] = 新数据

8，删除数据
var arr = ['张三','李四']  数组名.length = 1;

9，数组的遍历
for(var i = 0; i < arr.length; i++){
    console.log(arr[i])
}

10,多维数组
二维数组[[1,2],[3,4]]
三维数组[[[11,22],[33,44]]]
... 多维数组

11，flat() 可以把 n维数组转一堆数组
语法 ： 数组名.flat(5)
```



## 函数

```javascript
作用：封装代码，代码复用

语法：
function 函数名 = function(){
    //函数体
}
注意: 函数名建议动词开头，驼峰命名 getSum  getNum...

调用函数：  函数名();

实参： 函数名(实参1，实参2，实参3 ..... 实参n);

注意： 1,函数的形参和实参是一一对应的

arguments (伪数组)
解决的问题:当函数的形参个数不确定
通过arguments 拿到所有参数
arguments 是伪数组，可以遍历，可以通过下标访问数据

函数的返回值： return
1， return 任意数据类型
2， return 语句后面的代码都不执行

函数四要素：
//1.参数  2.返回值  3.功能 4.何时调用
function 函数名(形参1，形参2){
    return 数据；
}
```



## 对象

```javascript
作用： 保存多个信息

语法：
   1，字面量创建对象
   var 对象名 = {
       键名1：值1,
       键名2：值,
       ....
       键名n：值n;
   }
   
2，构造函数创建对象
 var 对象名 = new Object();
 对象名.属性名1 = 值1
 对象名.属性名2 = 值2

3，工厂函数创建对象
function create(参数1，参数2){
    var o = new Object();
    o.属性1 = 参数1；
    o.属性2 = 参数2；
    return o；
}
工厂函数：解决重复创建对象的问题，但是带来新问题，违法通过instancof来效验谁创建出的对象

4，自定义构造函数创建对象
function Creator(形参1，形参2){
    this.属性1 = 形参1;
    this.属性n = 形参n
}
自定义构造函数解决了  
通过instanceof效验对象实例是谁创建的

注意： 值可以是任意数据类型，可以是函数

访问对象属性：
  1，对象名.属性名
  2，对象名['属性名']
  3，对象名[变量]

拓展：对象解构
var {属性名1，属性名2....属性名n} = 对象名
```



## 内置对象

### Math对象

|            属性            |            功能            |
| :------------------------: | :------------------------: |
|          Math.Pl           |           圆周率           |
|        Math.floor()        |          向下取整          |
|        Math.ceil()         |          向上取整          |
|        Math.round()        |   四舍五入  （-3.5取-3）   |
|         Math.abs()         |           绝对值           |
| Math.max()   /  Math.min() |       求最大和最小值       |
|       Math.random()        | 获取范围在[0，1]内的随机值 |

注意：上面的方法使用时必须带括号

### 日期对象

- 使用Date实例化日期对象

```javascript
var now = new Date();
```

- Date实例的属性与方法

  | 方法名          | 说明                 |      |
  | --------------- | :------------------- | :--: |
  | getFullYear（） | 获取当年             |      |
  | getMonth（）    | 获取当月（0-11）     |      |
  | getDate（）     | 获取当天日期         |      |
  | getDay（）      | 获取星期（周0到周6） |      |
  | getHours（）    | 获取当前小时         |      |
  | getMinutes（）  | 获取当前分钟         |      |
  | getSeconds（）  | 获取当前秒钟         |      |

  毫秒数：

  ```javascript
  var date = new Data();
  
  *//  1,通过valueOf() 或 getTime();*
  
   console.log(date.valueOf());*//就是我们现在的时间*
  
   console.log(date.getTime());
  
  *//  2,简单的写法 (最常用的写法)*
  
  var date1 = +new Data();*// +new Data()  返回的就是总的毫秒数*
  
  *// 3，新方法*
  
  console.log(new Data());
  ```

  

## 数组常用方法



> - [ ] ```javascript
>   增加数据：
>   ```
> ```
> 
> 1，从前加
> var r =数组名.unshift(数据);
> 2，从后加
> var r =数组名.push(数据)；
> 
> 共性：
> 1，返回值r都是数组最新的长度
> 
> 区别：
> 1，unshift()从前加数据
> 2，push()从后添加数据（添加数据）
> 
> 注意：
> 1，push  可以从后添加多条数据
> 2，unshift  可以从前添加多条数据
> 
> 删除数据
> 1，从前删
> var r = 数组名.shift()
> 2，从后删
> var r = 数组名.pop()
> 
> 共性：
> 1，返回值是删除的数据
> 
> 区别：
>  1，shift() 从数组最前面删除一项数据
>  2，pop() 从数组最后一项删除数据
>  
> splice：实现增删改
> 
> 新增：数组名.splice(参数1开始操作的下标,0，参数3是新增数据)； 
> 删除：数组名.splice(参数1开始操作下标，大于0的正数)；
> 修改：数组名.splice(参数1开始操作的下标，大于0的正数，参数3新增的数据)；
> 
> 注意：
>  1，splice 会修改原数组
>  2，返回值是空数组（一般用不到）
>  
> slice:截取数组中的数据
> 数组名.slice(参数1，参数2)；
> 注意：
> 1,slice(参数)，下标从参数开始截取到数组最后一项
> 2,slice(参数1，参数2)截取数组，[参数，end] 包头不包尾
> 3,slice 不会修改原数组
> 
> 4，数组排序
> 1，reverse():颠倒数组中元素的顺序，无参数（翻转数组）
> 2，sort():对数组的元素进行排序
> （以上方法会改变原来的数组，返回新数组）
> `arr.sort = [1,12,3,77,5];
> return a - b; //升序排序
> return b - a; //降序 `
> 
> 5，数组转换为字符串
> 1，toString()：把数组装换成字符串，逗号分隔每一项
> 2， join('分隔符')：方法用于把数组中的所有元素转换为一个字符串
> （返回一个字符串）
> 3，concat（）：连接两个或多个数组，不影响原数组（返回一个新的数组）
> 
> 6，获取数组元素索引号 //如果有返回下标，没有则 返回-1；
> 从前查找：indexOf（'元素'）；
> 从后查找：lastIndexOf('元素')；
> ```
>



## 字符串对象

```javascript
· 字符串的不可变：
  指的是里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内存空间。
  
1，根据字符返回位置
   1，indexOf（'要查找的字符',开始的位置）：返回指定内容在元字符串中的位置，如果找不到就返回 - 1，开始的位置是index的索引号
   2，lastindexOf：从后往前找，只找到一个匹配
   
2，根据位置返回字符
   1，charAt(index)：返回指定位置的字符（index字符串的索引号）
   2，charCodeAt(index):获取指定位置打处字符的ASCLL码（index索引号）
   3，str[index]：获取指定位置处字符
   
 3，字符串操作方法
   1，concat(str1,str2,str3..):concat()方法用于连接两个或多个字符串。
   拼接字符串，等效于+，+更常用。
   2，substr(start,length):从start位置开始（索引号），length取的个数
   3，slice(start,end):从start位置开始，截取到end位置，end取不到（他们俩都是索    引号）
   4,subString(start,end):从start位置开始，截取到end位置，end取不到 ，基本和    slice相同 都是不接受负值
   5，replace('原','替');//把第一个参数替换为第二个参数；
   6，split('( 隔开的东西 数组用,)');//字符串装换为数组
   7，toUpperCase();//将所有的英文字符转换为大写字母
   8，toLowerCase();//将所有的大小字母转换为英文字符

get 获得 element 元素  by通过
```

 


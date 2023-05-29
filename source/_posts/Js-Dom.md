---
title: JavaScript - DOM
date: 2020-11-28 00:00:00
type: 
description: 'JSDOM文档树的解析'
tag: JavaScript
cover: https://s3.bmp.ovh/imgs/2021/12/4d4b264f31082964.png
---

# DOM

DOM文档树：

文档：一个页面就是一个文档，DOM中使用document表示节点；                                                                                  节点：网页中的所有内容，在文档树中都是节点（标签、属性、文本、注释等），使用node表示标签节点；                                                                                       标签节点：网页中的所有标签，通常称为元素节点，又简称为“元素”，使用element表示

# 获取元素

```javascript
1,根据id获取元素：document.getElementById('id')；//返回值：元素对象 或 null
2,根据标签获取元素：document.getElementsByTagName('标签')或element.getElementsByTagName('标签名')；  //返回值：元素对象集合
3,document.getElementsByClassName('类名') //根据类名返回元素对象集合;
4,document.querySelector('选择器')//根据指定选择器返回第一个元素对象;
5,document.querySelectorAll('选择器')//根据指定选择器返回;

· 特殊元素 ·
1,document.body; //返回元素对象
2,document.documentElement //返回html元素对象

· 操作元素 ·
1,element.innerText:从起始位置到终止位置的内容，但它去除html标签，同时空格和换行也会去掉
2,element.innerHTML:起始位置到终止位置的全部内容，包括html标签，同时保留空格和换行

1，· 元素属性操作 ·
innerText不会识别html，而innerHTML会识别
2, · innerText innerHTML src href id alt  title ·
获取属性的值 ： 元素对象.属性名
设置属性的值 ： 元素对象.属性名 = 值
3，· 利用DOM可以操作如下表单元素的属性·
type value checked selected disabled 
设置属性的值 ： 元素对象.属性名 = 值
表单元素中有一些属性如：disabled、checked、selected，元素对象的这些属性的值是布尔型
4，· 样式属性操作
element.style     行内样式操作
element.className 类名样式操作

// console.dir : 打印返回的元素对象，更好的查看里面的属性和方法 
```



# 鼠标事件

| 鼠标事件    | 触发条件         |
| :---------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |


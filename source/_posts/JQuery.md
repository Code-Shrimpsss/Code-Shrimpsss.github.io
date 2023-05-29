---
title: JQuery
date: 2020-05-26 00:00:00
type:
description: 'JQuery实用笔记'
cover: https://s3.bmp.ovh/imgs/2021/12/f8d71786cdaaac61.png
---

jQuery的官网地址： https://jquery.com

> 优先引入引入jQuery文件；

## jQuery中的顶级对象：$   (使用 jQuery 代替)

## jQuery 的入口函数

`jQuery中常见的两种入口函数`

```javascript
//第一种（简易）
$(function(){
   ...   //此处是页面 DOM 加载完成的入口 
});
//第二种（繁琐）
$(docuemnt).ready(function(){
   ...  
})
`好处：等着 DOM 结构渲染完毕即可执行内部代码，
 相当于原生 js 中的 DOMContentLoaded 。`
```

## jQuery对象与DOM对象转换

```javascript
`1，DOM对象转换成jQuery对象`
var box = document.getElementById('box');//获取DOM对象
var jQueryObject = $(box); //把DOM对象转换为 jQuery 对象
`2，jQuery对象转换为 DOM 对象有两种方法：`
//2.1 jQuery 对象[索引值]
var domObjOne = $('div')[0]
//2.2
var domObjTwo = $('div').get(0)
```

## JQuery 选择器

- #### 基础选择器

  ```javascript
  $('选择器')  //里面选择器总结写CSS选择器即可（加引导）
  ```

  

- #### 层级选择器之子代与后代

  ```javascript
  console.log($('ul li')); //子代选择器
  console.log($('ul>li')); //后代选择器
  ```

  

- #### 筛选选择器

  ```javascript
  $(function(){
      //（：first） 修改第一个元素的样式
      $("ul li:first").css("color","blue");
      //（eq（index））修改第index的元素的样式
      $("ul li:eq(2)").css("color","blue");
      //（：odd）修改当前所有偶数元素的样式
      $("ul li:odd").css("color","blue");
      //（：even）修改当前所有奇数元素的样式
      $("ul li:even").css("color","blue");
  })
  ```

  ## jQuery小方法

  - 隐式迭代

    ```javascript
    // 遍历内部 DOM 元素（伪数组形式存储）的过程就叫做隐式迭代。
    $('div').hide();  // 页面中所有的div全部隐藏，不用循环操作
    ```

    

  - 排他思想

    ```javascript
    // 想要多选一的效果，排他思想：当前元素设置样式，其余的兄弟元素清除样式。
    $(this).css(“color”,”red”);
    $(this).siblings(). css(“color”,””);
    ```

    

  - 链式编程

    ```javascript
    // 链式编程是为了节省代码量，看起来更优雅。
    $(this).css('color', 'red').sibling().css('color', ''); 
    ```

    ## 操作CSS样式方法

    ```javascript
    `1.参数只写属性名，则是返回属性值`
    var strColor = $(this).css('color');
    
    `2.  参数是属性名，属性值，逗号分隔，是设置一组样式，
    属性必须加引号，值如果是数字可以不用跟单位和引号`
    $(this).css(''color'', ''red'');
    
    ` 3.  参数可以是对象形式，方便设置多组样式。
    属性名和属性值用冒号隔开， 属性可以不用加引号`
    $(this).css({ "color":"white","font-size":"20px"});
    ```

## 设置类样式方法

```javascript
// 1.添加类
$("div").addClass("current");

// 2.删除类
$("div").removeClass("current");

// 3.切换类
$("div").toggleClass("current");

// 4.查询类
$("div").find("current");
```

## jQuery效果

```javascript
- 显示隐藏：show(显示) /  hide(隐藏)  /  toggle(切换) ;
- 滑入滑出：slideDown(下滑) / slideUp(上滑) / slideToggle(滑动切换) ; 
- 淡入淡出：fadeIn(淡入) / fadeOut(淡出) / fadeToggle(切换) / fadeTo(调整透明度) ; 
- 自定义动画：animate() ;
- 停止动画：stop();
```

​    

```javascript
`显示隐藏代码格式`
        $(function() {
            $("button").eq(0).click(function() {
                // show()  /  hide()  / toggole()
                $("div").show(1000, function() {
                    alert(1);
                });
            })
`滑入滑出代码格式`
        $(function() {
            $("button").eq(0).click(function() {
        // 下滑动 slideDown() / 上滑动 slideUp() / 滑动切换 slideToggle()
                $("div").slideDown();
            })
`淡出淡入代码格式`
        $(function() {
            $("button").eq(0).click(function() {
                // 淡入 fadeIn() / 淡出 fadeOut() / 淡入淡出切换 fadeToggle()
                $("div").fadeIn(1000);
                //  修改透明度 fadeTo() 这个速度和透明度要必须写
                $("div").fadeTo(1000, 0.5);
            })
`自定义动画代码格式`
            $(function() {
            $("button").click(function() {
                $("div").animate({
                    left: 500,
                    top: 300,
                    opacity: .4,
                    width: 500
                }, 500);
            })
        })
```



## jQuery属性操作 ##

==**jQuery 常用属性操作有三种：prop() / attr() / data()**==

1. ##### 元素固有属性值 prop()  #####

   ```javascript
   1. 获取属性语法： prop("属性")
   2. 设置属性语法： prop("属性","属性值")
   ```

   

2. ##### 元素自定义属性值 attr() #####

   ```javascript
   1.attr("属性")  //类似原生getAttribute
   2.attr("属性","属性值") //类似原生setAttribute
   ```

   

3. ##### 数据缓存 data()  #####

```javascript
1.data("name","value") //向被选元素附加数据
2.data("name")  //向被选元素获取数据
·还可以读取 HTML5 自定义属性  data-index ，得到的是数字型·
```

## jQuery文本属性值 ##

==**jQuery的文本属性值常见操作有三种：html() / text() / val()**==

 **分别对应JS中的 innerHTML 、innerText 和 value 属性**==

1. ##### 普通元素获取 html() （相当于原生 innerHTML） #####

   ```javascript
   html()       //获取元素的内容
   html("内容") //设置元素的内容
   ```

2. ##### 普通元素文本内容text() （相当与原生 innerHTML） #####

   ```javascript
   text()       //获取元素的文本内容
   text("文本内容")  //设置元素的文本内容
   ```

3. ##### 表单的值 val() （相当与原生 value） #####

```javascript
val()         //获取表单的值    
val("内容")   //设置表单的值
```

## jQuery元素操作 ##

==**jQuery 元素操作主要讲的是用jQuery方法，操作标签的遍历、创建、添加、删除等操作。**==

##### ！ 遍历 jQuery 对象中的每一项，回调函数中元素为 DOM 对象，想要使用 jQuery 方法需要转换 #####

1. ##### 遍历元素 #####

   ```javascript
   `方法一`
   $("div").each(function( index ,domEle){方法...})
   1. each()方法遍历匹配每一个元素。主要用DOM处理
   2. 回调函数有2个参数：index是每个元素的索引号；domEle是每个DOM元素对象，不是jquery对象
   
   `方法二`
   $.each(Object,function(index , element){方法...})
   1. $.each 可用于遍历任何对象。主要用于数据处理，比如数组，对象
   2. 里面的函数有2个参数：index 是每个元素的索引号；element 遍历内容
   ```

2. 创建，添加，删除

   ```javascript
   1. 创建
   $("<li></li>")
   2.内部添加  
   element.append("内容")  //把内容放入匹配元素内部最后面，类似原生appendChild
   element.prepend("内容") //把内容放入匹配元素内部最前面
   3.外部添加  
   element.after("内容")   //把内容放入目标元素后面
   element.before("内容")  //把内容放入目标元素前面
   `内部添加元素，生成之后，他们是父子关系`
   `外部添加元素，生成之后，他们是兄弟关系`
   4.删除元素
   element.remove() //删除匹配的元素(本身)
   element.empty()  //删除匹配的元素集合中所有的子节点
   element.html()   //清空匹配的元素内容
   `remove删除元素本身`
   `empt() 和  html("")作用等价，都可以删除元素里面的内容，只不过html还可以设置内容`
   ```

   

## jQuery尺寸 位置操作 ##

| 语法                             | 用法（取得匹配元素宽度和高度值） |
| -------------------------------- | -------------------------------- |
| width() / height()               | 只算width / height               |
| innerWidth() / innerHeight       | 包含 padding                     |
| outerWidth() / outerHeight       | 包含 padding ， border           |
| outerWidth(true)  /  outerHeight | 包含 padding ，border，margin    |

```javascript
`outerWidth()  / outerHeight()  获取设置元素 width和height + padding + border 大小 `
console.log($("div").outerWidth()); 
`outerWidth(true) / outerHeight(true) 获取设置 width和height + padding + border + margin`
console.log($("div").outerWidth(true));
//以上参数为空 ，则是获取相应值，返回的是数字型
//如果参数为数字，则是修改相应值 
//参数可以不必写单位
```

## jQuery位置操作 ##

==**jQuery的位置操作主要有三个： offset()、position()、scrollTop()/scrollLeft()** ==

- ##### offset() 设置或获取元素偏移 #####

1. offset() 方法设置或返回被选元素相对于文档的偏移坐标，跟父级没有关系。

2. offset().top 用于获取距离文档顶部的距离， offset().left 用于获取距离文档左侧的距离

3. 可以设置元素的偏移量：offset({left：10，left：30})

   ```javascript
   // 获取设置距离文档的位置（偏移）
   console.log($(".son").offset());
   console.log($(".son").offset().top);
   ```

- ##### position() 获取元素偏移 #####

1. position() 方法用于返回被选元素相对于带有定位的父级偏移坐标，如果父级都没有定位，则以文档为准。

2. offset().top 用于获取距离文档顶部的距离， offset().left 用于获取距离文档左侧的距离

3. 该方法只能获取

   ```javascript
   //获取距离带有定位父级位置（偏移） position  如果没有带有定位的父级，
   //则以文档为准 ; 这个方法只能获取不能设置偏移
   console.log($(".son").position());
   ```

- ##### scrollTop() / scrollLeft()  设置或获取元素被卷去的头部和左侧 #####

1. scrollTop()方法设置或返回被选元素被卷去的头部
2. 不跟参数是获取，参数为不带单位的数字则是设置被卷去的头部。

```javascript
// 被卷去的头部 scrollTop()  / 被卷去的左侧 scrollLeft()
$(document).scrollTop(100);
```

![](D:\学习资料\Jquery课件\day02\teach\images\总结.png)

## jQuery事件注册 ##

```javascript
        $(function() {      //  单个事件注册
            $("div").click(function() {
                $(this).css("background", "purple");
            });
        });
```

### jQuery事件基础 ###

- #### 页面事件 ####

  1. onload事件

     onload表示文档加载完成后再执行的一个事件

     ```javascript
     语法：
     window.onload = function(){};
     `在JavaScript中window.onload只能调用一次`
     ```

  2. `ready事件`

     ready表示文档加载完成后再执行的一个事件

     ```javascript
     语法：4种写法；
     $(document).ready(function(){...});
     jQuery(document).ready(function(){...});
     $(function(){...});  🉑推荐
     jQuery(function(){...});
     `$(document).ready()可以多次调用`
     ```

  3. select事件

     当我们选中’单行文本框‘或‘多行文本框’的内容时，就会触发select事件；

     ```JavaScript
     语法：
     $('#text').select(function(){alert('你选中了单(多)行文本框的内容')})；
     ```

  4. change事件

     change事件常用与‘具有多个选项的表单元素’中change事件在以下3种情况下触发；

     - 单选框选择某一项时触发

     - 复选框选择某一项时触发

     - 下拉列表选择某一项时触发

       ```JavaScript
       
       ```

  5. contextmenu事件

     常用的编辑事件只有一种，那就是contextmenu事件

     ```JavaScript
     场景：禁用鼠标右键
     $('body').contextmenu(function(){return false;})
     
     场景：点击鼠标右键切换背景颜色
     $('div').contextmenu(function(){$(this).css('background-color','skyblue')})
     ```

  6. sroll()滚动事件

     scroll()常和scrollTop()方法一起

     ```javascript
     语法：
     $().scroll(function(){...})
     ```

  7. 

  8. 

     

### jQuery事件处理 ###

- ##### on(): 用于事件绑定，目前最好用的事件绑定方法 #####

- ##### off(): 事件解绑 #####

- ##### trigger() / triggerHandler(): 事件触发 #####

  #### ==事件处理 on() 绑定事件== ####

  ```javascript
  `on 可绑定多个事件，多个事件处理程序`
  //事件
  $('div').on({     
      mouseover:function(){},
      click:function(){},
      blur:function(){}
  })；
  //如果事件处理程序相同 可以套用
  $('div').on('mouseover mouseout',function(){
      $(this).toggleClass('current');
  });
  //可以事件委派操作
  （把原来加给子元素身上的事件绑盯定在父元素身上，就是把事件委派给父元素）
  $('ul').on('click','li',function(){
      alert('Hello World!')
  })
  //on可以给未来动态的元素绑定事件
  $('ol').on('click','li',function(){
      alert(11);
  })
  var li = $('<li>我可以被替换哦</li>')
  $('ol').append(li);
  ```

  ### ==事假处理 off() 解绑事件== ###

#### off() 的方法可以移除通过 on() 方法添加的事件处理程序 ####

```javascript
$('p').off()          //解绑p元素首映事件处理程序
$('p').off('click')   //解绑p元素上面的点击事件 后面的foo 是侦听函数名
$('ul').off('click','li') //解绑事件委托

如果有的事件只想触发一次，可以使用 one() 来绑定事件
          // one() 但是它只能触发事件一次
          $("p").one("click", function() {
            alert(11);
          })
```

### ==事件处理trigger()自动触发事件== ###

```javascript
// 第一种：trigger()
element.click()         //第一种简写模式                                                                                                                                                                                                                                                                                           
element.trigger('type') //第二种自动触发模式
// 第二种：triggerHandler()
element.triggerHandler(type) //第三种自动触发模式
```

### ==jQuery事件对象== ###

```javascript
语法： element.on(events,[selector],function(event) {})
阻止默认行为：event.preventDefault() 或者 return false
阻止冒泡：event.stopPropagation()
```

## jQuery 拷贝对象 ##

```javascript
语法： $.extend([deep],target,object1,[objectN])
```

1. deep：如果设为true 为深拷贝 ，默认为false 浅拷贝
2. target：要拷贝的目标对象
3. object1：待拷贝第一个对象的对象
4. objectN：待拷贝第N个对象的对象
5. 浅拷贝目标对象引用的被拷贝的对象地址，修改目标对象==会影响==被拷贝对象
6. 深拷贝，前面加true，完全克隆，修改目标对象==不会影响==被拷贝对象

### ==jQuery多库共存== ###

==保证在旧有版本正常运行的情况下，新的功能使用新的jQuery版本实现，这种情况被称为，jQuery 多库共存==

``` javascript
语法：jQuery解决方案：
1. 把里面的$符号 统一改成jQuery。 比如jQuery('div')
2. jQuery 变量规定新的名称：$.noConflict()  var xx = $.noConflict();
```

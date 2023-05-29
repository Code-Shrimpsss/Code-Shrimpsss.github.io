---
title: nextTick
date: 2021-10-04 00:00:00
tag: Vue
description: "Vue之nextTick简述"
cover: https://s3.bmp.ovh/imgs/2021/12/999b31bf5d1bcb85.png
---

# nextTick #

- #### nexttick官方定义 ####

> Vue.nextTick([callback, context])
>
> 参数：
> 	{Function} [callback]
> 	{Object} [context]
>
> `在下次DOM更新循环结束之后执行延迟回调.`
> ` 在修改数据之后立即使用这个方法， 获取更新之后的DOM`

> `Vue.nextTick`用于延迟执行一段代码，它接受2个参数（回调函数和执行回调函数的上下文环境），如果没有提供回调函数，那么将返回`promise`对象。

- #### 代码示例 ####

```js
// 修改数据
vm.msg = 'Hello world'
// DOM 还没有更新
Vue.nextTick( function() {})

// 作为一个 Promise 使用 
Vue.nextTick()
 .then(function () {
  // DOM 更新了
})
```


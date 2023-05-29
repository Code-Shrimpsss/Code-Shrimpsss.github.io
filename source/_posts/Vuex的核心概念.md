---
title: Vuex的核心概念
date: 2021-01-05 00:00:00
tag: Vue
description: "Vuex - 简述四点一线"
cover: https://s3.bmp.ovh/imgs/2021/12/999b31bf5d1bcb85.png
---

![Vue](https://s3.bmp.ovh/imgs/2021/12/999b31bf5d1bcb85.png)

**What is vuex？**

- vuex 是实现组件全局状态(数据)管理的一种机制，可以方便的实现组件之间的数据共享.
- 只有组件之间共享的数据，才有必要存储到 vuex 中；对于组件中的私有数据，依旧存储在组件自身的 data 中即可。
- 官网：https://vuex.vuejs.org/zh/

## state

**State** 提供唯一的公共数据源，`所有共享的数据都要统一存放到Store的State中进行存储`

定义 State：

```vue
// 初始化vuex对象 const store = new Vuex.Store({ state: { // 管理数据 num: 0 }
})
```

组件访问 State 中数据的方法：

```vue
this.$store.state.全局数据名称 若是在template中使用免去this前缀
```

```vue
// 从vuex中导入 mapStare 函数 import { mapState } from 'vuex' //
将全局数据映射为当前组件的计算属性 computed:{ ...mapState([ 'count' ]) }
```

## Mutations

**Mutations**用于`变更Store中的数据`，同步地修改状态

`组件中语法`：$store.commit('方法名称',params)；

**注：**只有 mutations 里的函数有权修改 state 中的数据

**注：** mutations 里不能包含异步操作

```vue
// --- store-> index.js const store = new Vuex.Store({ state: { count:0 },
mutations:{ add(state,step){ //参数为 state数据源与自定义参数 //变更状态
state.count += step } } }) // --- 需要引用的组件 `方法一`：methods:{
方法(){this.$store.commit('方法名称如：(add)',3)} } `方法二`： //
从vuex中按需导入 mapMutations函数 // 映射为当前组件的methods函数 import {
mapMutations } from 'vuex' methods:{ ...mapMutation(['sub','subN']) }
```

## Action

Action 用于处理异步任务

`组件中语法`：$store.dispath('方法名')

**注：**在 Actions 中不能直接修改 state 中的数据，必须通过 context.commit 中触发才行

**注：**

```vue
// --- store-> index.js const store = new Vuex.Store({ state: { count:0 },
mutations:{ add(state,step){ state.count += step } }, actions:{
addAsync(context,step){ context.commit('mutatioins中的方法名',parems); } } }) //
--- 需要引用的组件 `方法一`： 用 this.$store.dispatch('方法') 使用方法
`方法二`： //从vuex中按需引入 mapActions函数 // 映射为当前组件的methods函数
import { mapActions } from 'vuex' methods:{ ...mapActions(['sub','subN']) }
```

## Getter

**Getter**用于对 Store 中的数据获取之前进行加工处理，可以理解为 state 的计算属性

`组件中语法`：$store.getters.方法()

- ```vue
  // --- store-> index.js const store = new Vuex.Store({ state: {
  listX:['H','e','l','l','o'] }, mutations:{...}, actions:{...}, getters:{
  getList(state,step){ //方法名 return state.listX.reverse();
  //这里state指向state数据源 } } }) // --- 需要引用的组件 `方法一`： 用
  this.$store.getters.getlist() 使用方法 `方法二`： //从vuex中按需引入
  mapGetters 函数 // 映射为当前组件的methods函数 import { mapGetters } from
  'vuex' methods:{ ...mapGetters(['getList']) }
  ```

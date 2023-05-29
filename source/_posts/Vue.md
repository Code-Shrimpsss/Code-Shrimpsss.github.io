---
title: VUE脚手架
date: 2020-08-17 00:00:00
type:
description: 'VUE模块化打地基'
tag: VUE
cover: https://s3.bmp.ovh/imgs/2021/12/999b31bf5d1bcb85.png
---


# VUE #

`Vue 是一套用于构建用户界面的**渐进式框架**。`

官方文档：https://v3.cn.vuejs.org/guide/introduction.html#vue-js-%E6%98%AF%E4%BB%80%E4%B9%88



#### 1.把@vue/cli模块包按到全局, 电脑拥有vue命令, 才能创建脚手架工程 ####

- 全局安装命令

```bash
yarn global add @vue/cli
# OR
npm install -g @vue/cli
```

```javascript
vue -V //查看vue版本
yarn add  @vue/cli   //创建项目启动服务
`项目名不能带大写字母，中文和特殊符号`
yarn create `项目名`  //创建脚手架
`进入脚手架项目下, 启动内置的热更新本地服务器`

cd `项目名`
// 启动
npm run serve 
# 或 
yarn serve
```

### `1.1@vue/cli自定义配置` ###

src并列处新建vue.config.js

```jsx
/* 覆盖webpack的配置 */
module.exports = {
  devServer: { // 自定义服务配置
    open: true, // 自动打开浏览器
    port: 3000 //自定义配置端口地址
  }
}
```

###  `2. @vue/cli 目录和代码分析` ###

> 目标: 讲解重点文件夹, 文件的作用, 以及文件里代码的意思

```bash
 vuecil-demo        # 项目目录
    ├── node_modules # 项目依赖的第三方包
    ├── public       # 静态文件目录
      ├── favicon.ico# 浏览器小图标
      └── index.html # 单页面的html文件(网页浏览的是它)
    ├── src          # 业务文件夹
      ├── assets     # 静态资源
        └── logo.png # vue的logo图片
      ├── components # 组件目录
        └── HelloWorld.vue # 欢迎页面vue代码文件 
      ├── App.vue    # 整个应用的根组件
      └── main.js    # 入口js文件
    ├── .gitignore   # git提交忽略配置
    ├── babel.config.js  # babel配置
    ├── package.json  # 依赖包列表
    ├── README.md    # 项目说明
	└── yarn.lock    # 项目包版本锁定和缓存地址
```

- 主要文件及含义

```js
node_modules下都是下载的第三方包
public/index.html – 浏览器运行的网页
src/main.js – webpack打包的入口文件
src/App.vue – vue项目入口页面
package.json – 依赖包列表文件
```

### `3.el:挂载点` ###

> 必须在el命中的元素内部定义 （建议使用id选择器>
> 无法挂载到html 与 body 上

### `4._@vue/cli 单vue文件讲解` ###

> 推荐采用.vue文件来开发项目-单vue文件独立模块, 作用域互不影响
>
> template里只能有一个根标签
>
> style配合scoped属性, 保证样式只针对当前template内标签生效
>
> vue文件配合webpack, 把他们打包起来插入到index.html

```vue
<!-- template必须, 只能有一个根标签, 影响渲染到页面的标签结构 -->
<template>
  <div>欢迎使用vue</div>
</template>

<!-- js相关 -->
<script>
export default {
  name: 'App'
}
</script>

<!-- 当前组件的样式, 设置scoped, 可以保证样式只对当前页面有效 -->
<style scoped>
</style>

```

- 最终: Vue文件配合webpack, 把以上打包起来插入到index.html, 才可在浏览器运行

- assets 和 components 文件夹下的一切都删除掉 (不要默认的欢迎页面)
---
title: CSS3 - flex
date: 2021-05-02 00:00:00
type:
description: 'flex布局实用笔记'
cover: https://s3.bmp.ovh/imgs/2021/12/e59946e1568683f6.png
---


# Flex #

Flex是Flexible Box的缩写，意为”弹性布局”，用来为盒状模型提供最大的灵活性。

`任何一个容器都可以指定为Flex布局。`

```javascript
.box{
 display:flex;
}
```

### 容器的属性 ###

- **flex-direction**

  flex-direction属性决定主轴的方向

  ```javascript
  .box {
    flex-direction: row | row-reverse | column | column-reverse;
  }
  · row（默认值）：主轴为水平方向，起点在左端。
  · row-reverse：主轴为水平方向，起点在右端。
  · column：主轴为垂直方向，起点在上沿。
  · column-reverse：主轴为垂直方向，起点在下沿。
  ```

- **flex-wrap**

  felx-warp属性是决定换行

  ```javascript
  .box{
    flex-wrap: nowrap | wrap | wrap-reverse;
  }
  nowrap（默认）：不换行。
  wrap：换行，第一行在上方。
  wrap-reverse：换行，第一行在下方。
  ```

- **flex-flow**

  flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

  ```javascript
  .box {
    flex-flow: <flex-direction> || <flex-wrap>;
  }
  ```

- **justify-content**

  flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

  ```javascript
  .box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
  }
  
  · flex-start（默认值）：左对齐
  · flex-end：右对齐
  · center： 居中
  · space-between：两端对齐，项目之间的间隔都相等。
  · space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
  ```

- **align-items**

  align-items属性定义项目在交叉轴上如何对齐。

  ```javascript
  .box {
    align-items: flex-start | flex-end | center | baseline | stretch;
  }
  
  · flex-start：交叉轴的起点对齐。
  · flex-end：交叉轴的终点对齐。
  · center：交叉轴的中点对齐。
  · baseline: 项目的第一行文字的基线对齐。
  · stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
  ```

- **align-content**

  align-content属性定义了多根轴线的对齐方式

  ```javascript
  .box {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
  }
  
  · flex-start：与交叉轴的起点对齐。
  · flex-end：与交叉轴的终点对齐。
  · center：与交叉轴的中点对齐。
  · space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
  · space-around：每根轴线两侧的间隔都相等。
  · stretch（默认值）：轴线占满整个交叉轴。
  ```

  

- order属性

  order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

  ```javascript
  .item {
    order: <integer>;
  }
  ```

- flex-grow属性

  flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大

  ```javascript
  .item {
    flex-grow: <number>; /* default 0 */
  }
  ```

- flex-shrink属性

  flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小

  ```javascript
  .item {
    flex-shrink: <number>; /* default 1 */
  }
  ```

- flex-basis属性

  flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。默认值为auto

  ```javascript
  .item {
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
  }
  ```

- align-self属性

  align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。auto

  ```javascript
  .item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
  }
  ```


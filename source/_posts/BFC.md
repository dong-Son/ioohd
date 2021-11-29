---
title: 盒模型
date: 2019-11-06
tags: css、BFC
---

### 什么是 BFC

BFC(Block formatting context)译为”块格式化上下文”，是一个独立的渲染区域，规定了内部的子元素如何布局，并且与这个区域外部毫不相干。

<!--more--> 

### 哪些元素会生成 BFC

1. 根元素 html
2. float 属性不为 none
3. position 为 absolute 或 fixed
4. display 为 inline-block, table-cell, table-caption, flex, inline-flex
5. overflow 不为 visible（除非该值已经被传播到视口）

### BFC 布局规则是怎样的

1. 内部的 Box 会在垂直方向，一个接一个地放置。
2. Box 垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
3. 每个元素的 margin box 的左边， 与包含块 border box 的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4. BFC 的区域不会与 float box 重叠。
5. BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算 BFC 的高度时，浮动元素也参与计算
7. 并不是只有块元素才会生成 BFC，任何元素只要符合条件即可生成 BFC

### BFC 的应用

1. 自适应两栏布局
2. 清除内部浮动
3. 防止 margin 上下重叠

---

本文作者： 一只野生东子
---
title: vue开发中需要注意的一些内容
date: 2020-06-13
tags: vue
---

### 主要用于记录在 Vue 项目开发中踩过的坑，或者没有了解过的东西(持续更新)

前端学无止境 ヾ(ﾟ ∀ ﾟゞ)
秃头从未停止 ヾ(ﾟ ∀ ﾟゞ)

<!--more-->

### v-if 和 v-show 要学会区分开使用

### 区别

1. 手段：v-if 是通过控制 dom 节点的存在与否来控制元素的显隐;v-show 是通过设置 DOM 元素的 display 样式，block 为显示，none 为隐藏;
2. 编译过程：v-if 切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件;v-show 只是简单的基于 css 切换;
3. 编译条件：v-if 是惰性的，如果初始条件为假，则什么也不做;只有在条件第一次变为真时才开始局部编译（编译被缓存?编译被缓存后，然后再切换的时候进行局部卸载); v-show 是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且 DOM 元素保留;
4. 性能消耗：v-if 有更高的切换消耗;v-show 有更高的初始渲染消耗;

### 使用场景

基于以上区别，因此，如果需要非常频繁地切换，则使用 v-show 较好;如果在运行时条件很少改变，则使用 v-if 较好。

### 总结

v-if 判断是否加载，可以减轻服务器的压力，在需要时加载,但有更高的切换开销;v-show 调整 DOM 元素的 CSS 的 dispaly 属性，可以使客户端操作更加流畅，但有更高的初始渲染开销。如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

### 页面内监听 scroll 事件的方法

如果是整个网页可以滑动，可以使用以下事件监听

```js
window.addEventListener("scroll", function () {
  console.log("scrolling");
});
```

但是大部分的场景是在一个元素内发生页面滚动

举例：在页面内的一个div需要监听的

* 滑动的组件外层的div加 ref="viewBox" 为了通过$refs获取dom元素

```html
<!--设备列表-->
<div class="deviceWrapper" ref="viewBox">
 <mu-refresh-control :refreshing="refreshing" :trigger="trigger" @refresh="refresh"/>
 <div class="demo-grid">
 <!--设备列表 手机一行两列 pad一行4列-->
 <mu-row>
  <mu-col v-for="device in devicesList" width="50" tablet="25" desktop="25">
  <deviceCardView :device-data="device""></devicelightCardView>
  </mu-col>
 </mu-row>
 </div>
 <p class="bottomLine" v-bind:class="{bottomLineVisible:isScroll}">---------------------我是有底线的---------------------</p>
</div>
```

* 获取元素，监听事件

```js
mounted() {
// 通过$refs获取dom元素
 this.box = this.$refs.viewBox
 // 监听这个dom的scroll事件
 this.box.addEventListener('scroll', () => {
 console.log(" scroll " + this.$refs.viewBox.scrollTop)
 //以下是我自己的需求，向下滚动的时候显示“我是有底线的（类似支付宝）”
 this.isScroll=this.$refs.viewBox.scrollTop>0
 }, false)
}
``` 

这样能正常打印出来.scrollTop

### 封装一个组件，用于段落文字点击展开和收起功能

### 组件调用方法

```html
<Unfold
  data="这里是文章测试标题，含标点一共二十个字。这里是文章测试标题，含标点一共二十个字。这里是文章测试标题，含标点一共二十个字。这里是文章测试标题，含标点一共二十个字。这里是文章测试标题，含标点一共二十个字。这里是文章测试标题，含标点一共二十个字。这里是文章测试标题，含标点一共二十个字。这里是文章测试标题，含标点一共二十个字。"
  maxLength="100"
/>
``` 

属性说明

```data```：文字数据
```maxLen```：最大长度，超过这个数，被截取，默认 80，不改长度无需传值

### 封装好组件的完整代码

需要此功能的地方直接调用这个组件

```html
<template>
  <span>
    <span v-if="data.length < maxLen">
      <span class="tj">{{ data }}</span>
    </span>
    <span v-else>
      <span class="tj"
        >{{ showBtn ? sliceStr : data }}
        <span class="btnWord" @click="showBtn = !showBtn">{{
          showBtn ? "全文" : "收起"
        }}</span>
      </span>
    </span>
  </span>
</template>

<script>
export default {
  name: "unfold",
  data() {
    return {
      showBtn: true,
    };
  },
  props: {
    // 数据
    data: {
      type: String,
      default: "",
    },
    // 最大长度
    maxLen: {
      type: Number,
      default: 80,
    },
  },
  computed: {
    sliceStr() {
      if (this.data != null) {
        return this.data.substring(0, this.maxLen) + "...";
      }
      return "";
    },
  },
};
</script>

<style lang="less" scoped>
.tj {
  text-align: justify;
}

.btnWord {
  color: cornflowerblue;
  cursor: pointer;
  word-break: keep-all;
}
</style>
``` 

---

本文作者： 一只野生东子
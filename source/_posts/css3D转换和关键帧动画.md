---
title: css3D转换和关键帧动画
date: 2019-10-22
tags: css
---

### css 3D 转换

CSS3 3D 转换 transform(3D 位移、缩放、旋转)

让元素方式 3d 的变换，同 2d 变换一样，通过 transform 来设置
**位移**：translate3D(x,y,z)
**缩放**：scale3d(x,y,z) scalez()不能单独使用；
**旋转**：rotatex(80deg) rotatey(80deg) rotatez(80deg)

<!--more--> 

转换是使元素改变形状、尺寸和位置的一种效果。

可以使用 2D 或 3D 转换来转换元素。

在转换概念当中：是没有 display 这么一说的，
通过改变元素的透明度去实现从无到有

IE 10 和 Firefox 支持 3D 转换。

Chrome 和 Safari 需要前缀 -webkit-。

Opera 仍然不支持 3D 转换（它只支持 2D 转换）。

1. 观察的场所—-3D 空间 transform-style
   transform-style:preserve-3d;   表示所有子元素在 3D 空间呈现
2. 近大远小—-景深 perspective
   元素距离观察点的距离（物体和眼睛的距离越小，近大远小的效果越明显）
   perspective:1200px;(在父元素中使用）
   通常的数值在 900-1200 之间，如果当你的视线距离物体足够远的时候，基本上就不会有近大远小的感觉
3. 观察 3D 元素的（位置）角度—-景深的角度perspective-origin
   perspective-origin:left top （左上角）

### 这三种写法是等价
```css
transform: translate3d(30px,30px,800px )

transform:translateZ(800px)  translateX(30px)  translateY(30px) ;

transform:translateZ(800px) translate(30px,30px);
```

### preserve-3d放到父元素

```css
-webkit-transform-style: preserve-3d;
-moz-transform-style: preserve-3d;
-ms-transform-style: preserve-3d;
-o-transform-style: preserve-3d;
transform-style: preserve-3d;
```

transform 向元素应用 2D 或 3D 转换。
transform-origin 允许你改变被转换元素的位置。
transform-style 规定被嵌套元素如何在 3D 空间中显示。
perspective 规定 3D 元素的透视效果。
perspective-origin 规定 3D 元素的底部位置。
backface-visibility 定义元素在不面对屏幕时是否可见

### 2D Transform 方法

matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n) 定义 3D 转换，使用 16 个值的 4x4 矩阵。
translate3d(x,y,z) 定义 3D 转化。
translateX(x) 定义 3D 转化，仅使用用于 X 轴的值。
translateY(y) 定义 3D 转化，仅使用用于 Y 轴的值。
translateZ(z) 定义 3D 转化，仅使用用于 Z 轴的值。
scale3d(x,y,z) 定义 3D 缩放转换。
scaleX(x) 定义 3D 缩放转换，通过给定一个 X 轴的值。
scaleY(y) 定义 3D 缩放转换，通过给定一个 Y 轴的值。
scaleZ(z) 定义 3D 缩放转换，通过给定一个 Z 轴的值。
rotate3d(x,y,z,angle) 定义 3D 旋转。
rotateX(angle) 定义沿 X 轴的 3D 旋转。
rotateY(angle) 定义沿 Y 轴的 3D 旋转。
rotateZ(angle) 定义沿 Z 轴的 3D 旋转。
perspective(n) 定义 3D 转换元素的透视视图。

### CSS3 动画

通过 CSS3，我们能够创建动画，
这可以在许多网页中取代动画图片、Flash 动画以及 JavaScript

1. CSS3 @keyframes 规则
   如需在 CSS3 中创建动画，需要学习 @keyframes 规则。
   @keyframes 规则用于创建动画。
   在 @keyframes 中规定某项 CSS 样式，
   就能创建由当前样式逐渐改为新样式的动画效果。
   Internet Explorer 10、Firefox 以及 Opera 支持 @keyframes 规则和 animation 属性。
   Chrome 和 Safari 需要前缀 -webkit-。

注释：Internet Explorer 9，以及更早的版本，不支持 @keyframe 规则或 animation 属性。

2. 使用方法

第一步：
```css
@-webkit-keyframes  动画名称{
    0%｛ 本关键帧中的样式｝
    10%｛ 本关键帧中的样式｝
      ......
    100%｛ 本关键帧中的样式｝
}
```

第二步：动画设计好后，使用 animation 属性调用动画。方法如下：
```css
想加动画元素的选择器{ -webkit-animation:  move 2s 3s linear 2 normal ｝
  /* 动画名称  持续时间   延迟时间       运动形式     播放次数  是否倒放*/
```

### 关键帧语法
```css
@keyframes name {
  from {
    properties: Properties value;
  }
  to {
    properties: Properties value;
  }
}
```

样例
```css
@-webkit-keyframes move {
    0% {
      margin-left: 100px;
      background: green;
    }
    40% {
      margin-left: 150px;
      background: orange;
    }
    60% {
      margin-left: 75px;
      background: blue;
    }
    100% {
      margin-left: 100px;
      background: red;
    }
}
```
```css
-webkit-animation-name: move; /*动画属性名，前面keyframes样例定义的动画名*/
-webkit-animation-duration: 10s; /*动画持续时间*/
-webkit-animation-timing-function: ease-in-out; /*动画帧频，和transition-timing-function是一样的*/
/*ease | linear | ease-in | ease-out | cubic-Bezier (n1 , n2, n3, n4)*/
-webkit-animation-delay: 2s; /*动画延迟时间*/
-webkit-animation-iteration-count: 10; /*动画循环次数，infinite为无限次*/
-webkit-animation-direction: normal; /*定义动画播放方式*/
/*默认normal，动画正常播放；  alternate，动画轮流反向播放*/
```

3. 实现动画的方法：
   A、linear：从开始到结束都是以同样的速度进行.
   B、ease-in：开始速度很慢，然后沿着曲线进行加快.
   C、ease-out：开始速度很快，然后沿着曲线进行减速.
   D、ease：开始时速度很快，然后沿着曲线进行减速，然后再沿着曲线加速.
   E、ease-in-out：开始时速度很慢，然后沿着曲线进行加速，然后再沿着曲线减速.
   F、step-start; /_ 马上跳到动画每一结束桢的状态 _/

### CSS3 动画属性

@keyframes 规定动画。
animation 所有动画属性的简写属性，除了 animation-play-state 属性。
animation-name 规定 @keyframes 动画的名称。
animation-duration 规定动画完成一个周期所花费的秒或毫秒。默认是 0。
animation-timing-function 规定动画的速度曲线。默认是 “ease”。
animation-delay 规定动画何时开始。默认是 0。
animation-iteration-count 规定动画被播放的次数。默认是 1。
animation-direction 规定动画是否在下一周期逆向地播放。默认是 “normal”。
animation-play-state 规定动画是否正在运行或暂停。默认是 “running”。
animation-fill-mode 规定对象动画时间之外的状态。

---
本文作者： 一只野生东子
---
title: 事件Event
date: 2019-12-11
tags: javaScript
---

### 事件基础

JavaScript 事件是由访问 Web 页面的用户引起的一系列操作。
当用户执行某些操作的时候，再去执行一系列代码。或者用来获取事件详细信息，如鼠标位置、键盘按键等。

### 事件处理函数

javaScript 可以处理的事件类型为：鼠标事件、键盘事件、HTML 事件
所有的事件处理函数都会都有两个部分组成，on + 事件名称

<!--more--> 

### 鼠标事件
#
onclick:用户单击鼠标按钮
ondblclick:当用户双击主鼠标按钮时触发
onmousedown:当用户按下鼠标还未弹起时触发
onmouseup：当用户释放鼠标按钮时触发
onmouseover：当鼠标移到某个元素上方时触发
onmouseout：当鼠标移出某个元素上方时触发
onmousemove：当鼠标指针在元素上移动时触发

### HTML 事件

onload：当页面或者资源完全加载后在 window 上面触发
onselect：当用户选择文本框(input 或 textarea)中的一个或多个字符触发
onchange：当文本框(input 或 textarea)内容改变且失去焦点后触发
onfocus：当页面或者元素获得焦点时在 window 及相关元素上面触发
onblur：当页面或元素失去焦点时在 window 及相关元素上触发
onresize：当窗口或框架的大小变化时在 window 或框架上触发
onscroll：当用户滚动带滚动条的元素时触发

### 事件对象

当触发某个事件时，会产生一个事件对象，这个对象包含着所有与事件有关的信息 。包括导致事件的元素、事件的类型、以及其它与特定事件相关的信息。

通过事件绑定的执行函数是可以得到一个隐藏参数的 。说明，浏览器会自动分配一个参数，这个参数其实就是 event 对象。

Event 对象获取方式 （兼容性）

```js
el.onclick = function (evt) {
  let e = evt || window.event;
};
```

event.button 属性
当前事件触发时哪个鼠标按键被点击
clientX、clientY 属性
鼠标在可视区 X 坐标和 Y 坐标，即距离左边框和上边框的距离
screenX、screenY 属性
鼠标在屏幕区 X 坐标和 Y 坐标，即距离左屏幕和上屏幕的距离
offsetX、offsetY 属性
鼠标相对于事件源的 X 坐标和 Y 坐标
pageX、pageY
鼠标相对于文档的 X 坐标和 Y 坐标

### 键盘事件

onkeydown：当用户按下键盘上任意键触发，如果按住不放，会重复触发
onkeypress：当用户按下键盘上的字符键触发，如果按住不放，会重复触发
onkeyup：当用户释放键盘上的键触发
组合键 ctrkey、altkey、shiftkey
altKey 属性，bool 类型，表示发生事件的时候 alt 键是否被按下
ctrlKey 属性，bool 类型，表示发生事件的时候 ctrl 键是否被按下
shiftKey 属性，bool 类型，表示发生事件的时候 shift 键是否被按下  
keyCode/which 兼容
事件源（事件在哪个元素上产生）

### 冒泡

1. 事件的冒泡
   事件按照从最特定的事件目标到最不特定的事件目标(document 对象)的顺序触发。
2. 阻止事件冒泡

```js
//阻止事件冒泡
e.stopPropagation();
//低版本ie
e.cancelBubble = true;
```

### 事件默认行为及阻止方式

1. 浏览器的默认行为
   JavaScript 事件本身所具有的属性，例如 a 标签的跳转，Submit 按钮的提交，右键菜单，文本框的输入等。
2. 阻止默认行为的方式
   w3c 的方法是e.preventDefault()，

IE 则是使用
e.returnValue = false;

return false;

自定义右键菜单 oncontextmenu

### DOM2 级事件处理程序

添加事件监听器：ele.addEventListener(事件名,处理函数,布尔值)
现代浏览器（IE9、10、11 | ff, chorme, safari, opera）
注意：事件名不带 on，处理函数为函数引用，布尔值代表冒泡(内到外)或捕获（外到内）

```js
element.addEventListener("click", function () {}, false); //false 事件冒泡
element.addEventListener("click", function () {}, true); //true事件捕获
```

移除事件监听器：```removeEventListener(事件名,处理函数)```

IE8 及以下的事件监听器：```attachEvent(事件名,处理函数)，detachEvent(事件名,处理函数)```
注意：事件名带 on

### 事件委托

利用事件冒泡的原理，把本应添加给某元素上的事件委托给他的父级（外层）。

使用案例
如果一个 ul 中有很多 li，循环遍历所有的 li，给 li 添加事件效率比较低，我们可以监听 ul 的点击事件，利用子元素的点击事件都会冒泡到父元素的特点，就可以知道什么时候点击了 li。

好处：效率高，可以给未来元素添加事件

### 事件对象中的拖拽效果

拖拽原理
三个事件：onmousedown、onmousemove、onmouseup

实现思路：

1. 给目标元素添加 onmousedown 事件，拖拽的前提是在目标元素按下鼠标左键。
2. 当 onmousedown 发生以后，此刻给 document 添加 onmousemove 事件，意味着此刻鼠标在网页的移动都将改变目标元素的位置。
3. 在 onmousemove 事件中，设定目标元素的 left 和 top，
   公式：
   目标元素的 left = 鼠标的 clientX - (鼠标和元素的横坐标差，即 offsetX)
   目标元素的 top = 鼠标的 clientY - (鼠标和元素的纵坐标差，即 offsetY)
4. 当 onmousedown 发生以后，此刻给 document 添加 onmouseup 事件，意味着此刻鼠标在网页的任意位置松开鼠标，都会放弃拖拽的效果。
5. 在 onmouseup 事件中，取消 document 的 onmousemove 事件即可。

--- 

本文作者： 一只野生东子
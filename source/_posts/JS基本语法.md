---
title: JS基本语法
date: 2019-11-30
tags: javaScript
---

### JavaScript 基本介绍及发展趋势

ECMAScript
通过 ECMA-262 标准化的脚本程序语言，JavaScript 是其实现和扩展。
1999 年 ES3 发布，成为 JavaScript 的通行标准。
2009 年 ES5 发布，在所有现代浏览器中得到了相当完全的实现
2015 年 ES6 发布，被部分实现于大部分现代浏览器

<!--more--> 

原生应用 web 应用 （跨平台） app
JavaScript 概念
JavaScript 是基于对象和事件驱动，并具有安全性能的客户端脚本语言，弱类型的语言。
解释执行 （编译-》代码转为 0 和 1）
由三部分组成：
ECMAScript（核心）
DOM（文档对象模型）document object model
BOM（浏览器对象模型）browser object model
高级编程， 面向对象编程

### JavaScript 和 HTML5 的关系

HTML5 的新特性
新的内容元素
新的表单控件
媒介元素
绘画
本地存储

广义的 HTML5 是包括 HTML、CSS、JavaScript 在内的一套技术组合
狭义的 HTML5 更多的是指 JavaScript.

### 编写 JS 及如何运行 JS

1. 在 HTML 标签中直接写入 JS 代码（用的少）

```<div id='div1' onclick='alert(“你好”)'>点击</div>```

2. 在 HTML 文档中写入代码
```<script></script>```

3. 在*.js 文件中写入 JS 代码(工作中常用)
```<script src=”a.js”></script>```

4. 注释
  //
  /**/

### 变量及命名规则

1. 变量的声明和定义
   var a = 10;
   var 是关键字， a 是变量名， =是赋值符号 10 是值
2. 变量的命名规则
   变量是由数字、字母、下划线（_）和美元符号（$）的一种或者几种组成，且不能以数字开头,严格区分大小写。
  （驼峰法则，见名知义）
3. 关键字
   ECMAScript 描述了一组特定用途的关键字，不能用作变量名，例如：If else do while for in 等。

### 算术、赋值、关系运算符

算术运算符

```js
+	 -    *    /      %

```

赋值运算符

```js
=   +=  -=  *=   /=  %=
```

关系运算符

```js
>   <   >=   <=   ==  !=    ===   !==
```

var num = 3.11111
num.toFixed(3)// toFixed(n) 保留 n 位小数

### 数据类型及类型转换

JavaScript 数据类型
数值、字符串、布尔、undefined、null、对象

JavaScript 类型转换
隐式转换
显式转换

### 逻辑运算符

逻辑与 && and

逻辑或 || or

逻辑非 ！ not

### 自增自减运算

自增++
自减–

### 八进制和十六进制

八进制 071
十六进制 0x12

### Number 方法的应用，NaN

NaN(not a number):不是一个数字
Number()：将值转化为数字

---

本文作者： 一只野生东子
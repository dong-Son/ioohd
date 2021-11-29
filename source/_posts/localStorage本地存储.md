---
title: localStorage本地存储
date: 2020-05-15
tags: javaScript
---

### 什么是 localStorage

在 HTML5 中，新加入了一个 localStorage 特性，这个特性主要是用来作为本地存储来使用的，
解决了 cookie 存储空间不足的问题(cookie 中每条 cookie 的存储空间为 4k)，
localStorage 中一般浏览器支持的是 5M 大小，这个在不同的浏览器中 localStorage 会有所不同

<!--more--> 

### localStorage 的优势

1. localStorage 拓展了 cookie 的 4K 限制
2. localStorage 会可以将第一次请求的数据直接存储到本地，这个相当于一个 5M 大小的针对于前端页面的数据库，相比于 cookie 可以节约带宽，但是这个却是只有在高版本的浏览器中才支持的

### localStorage 的局限

1. 浏览器的大小不统一，并且在 IE8 以上的 IE 版本才支持 localStorage 这个属性
2. 目前所有的浏览器中都会把 localStorage 的值类型限定为 string 类型，这个在对我们日常比较常见的 JSON 对象类型需要一些转换

### localStorage 的写入

localStorage 的写入有三种方法。 localStorage 只支持 string 类型的存储

```js
var storage = window.localStorage;
//写入a字段
storage["a"] = 1;
//写入b字段
storage.a = 1;
//写入c字段
storage.setItem("c", 3);
```

### 三种对 localStorage 的读取

其中官方推荐的是 getItem\setItem 这两种方法对其进行存取

```js
//第一种方法读取
var a = storage.a;
console.log(a);
//第二种方法读取
var b = storage["b"];
console.log(b);
//第三种方法读取
var c = storage.getItem("c");
console.log(c);
``` 
### localStorage 的修改

改这个步骤比较好理解，思路跟重新更改全局变量的值一样

```js
var storage = window.localStorage;
//写入a字段
storage["a"] = 1;
storage.a = 4; //修改
console.log(storage.a);
``` 

### localStorage 的删除

将 localStorage 中的某个键值对删除

```js
storage.setItem("c", 3);
console.log(storage);
storage.removeItem("a");
```

### 将 localStorage 的所有内容清除

```js
storage.clear();
```

### localStorage 其他注意事项

一般我们会将 JSON(js 中的对象)存入 localStorage 中，但是在 localStorage 会自动将 localStorage 转换成为字符串形式
这个时候我们可以使用 JSON.stringify()这个方法，来将 JSON 转换成为 JSON 字符串

```js
var data = {
  name: "zhangsan",
  sex: "man",
};
var d = JSON.stringify(data);
storage.setItem("data", d);
//将JSON字符串转换成为JSON对象输出
var json = storage.getItem("data");
var jsonObj = JSON.parse(json);
```

---

本文作者： 一只野生东子
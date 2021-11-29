---
title: 关于ES5和字符串string
date: 2019-11-15
tags: js
---

### ECMAScript 发展史

ECMAScript 是一种由 ECMA 国际（前身为欧洲计算机制造商协会,英文名称是 European Computer Manufacturers Association）通过 ECMA-262 标准化的脚本程序设计语言。这种语言在万维网上应用广泛，它往往被称为 JavaScript 或 JScript，所以它可以理解为是 javascript 的一个标准,但实际上后两者是 ECMA-262 标准的实现和扩展。

<!--more--> 

1998 年 6 月，ECMAScript 2.0 版发布。
1999 年 12 月，ECMAScript 3.0 版发布，成为 JavaScript 的通行标准，得到了广泛支持。
2007 年 10 月，ECMAScript 4.0 版草案发布，对 3.0 版做了大幅升级，预计次年 8 月发布正式版本。草案发布后，由于 4.0 版的目标过于激进，各方对于是否通过这个标准，发生了严重分歧。以 Yahoo、Microsoft、Google 为首的大公司，反对 JavaScript 的大幅升级，主张小幅改动；以 JavaScript 创造者 Brendan Eich 为首的 Mozilla 公司，则坚持当前的草案。
2008 年 7 月，由于对于下一个版本应该包括哪些功能，各方分歧太大，争论过于激进，ECMA 开会决定，中止 ECMAScript 4.0 的开发，将其中涉及现有功能改善的一小部分，发布为 ECMAScript 3.1，而将其他激进的设想扩大范围，放入以后的版本，由于会议的气氛，该版本的项目代号起名为 Harmony（和谐）。会后不久，ECMAScript 3.1 就改名为 ECMAScript 5。
2009 年 12 月，ECMAScript 5.0 版正式发布。Harmony 项目则一分为二，一些较为可行的设想定名为 JavaScript.next 继续开发，后来演变成 ECMAScript 6；一些不是很成熟的设想，则被视为 JavaScript.next.next，在更远的将来再考虑推出。
2011 年 6 月，ECMAscript 5.1 版发布，并且成为 ISO 国际标准（ISO/IEC 16262:2011）。
2013 年 3 月，ECMAScript 6 草案冻结，不再添加新功能。新的功能设想将被放到 ECMAScript 7。
2013 年 12 月，ECMAScript 6 草案发布。然后是 12 个月的讨论期，听取各方反馈。
2015 年 6 月 17 日，ECMAScript 6 发布正式版本，即 ECMAScript 2015。

### ES5 严格模式

### 严格模式

除了正常的运行模式，JavaScript 还有第二种运行模式：严格模式（strict mode）。顾名思义，这种模式采用更加严格的 JavaScript 语法。
同样的代码，在正常模式和严格模式中，可能会有不一样的运行结果。一些在正常模式下可以运行的语句，在严格模式下将不能运行。

### 设计目的

早期的 JavaScript 语言有很多设计不合理的地方，但是为了兼容以前的代码，又不能改变老的语法，只能不断添加新的语法，引导程序员使用新语法。
严格模式是从 ES5 进入标准的，主要目的有以下几个。

1. 明确禁止一些不合理、不严谨的语法，减少 JavaScript 语言的一些怪异行为。
2. 增加更多报错的场合，消除代码运行的一些不安全之处，保证代码运行的安全。
3. 提高编译器效率，增加运行速度。
4. 为未来新版本的 JavaScript 语法做好铺垫。
总之，严格模式体现了 JavaScript 更合理、更安全、更严谨的发展方向

### 启用方法

进入严格模式的标志，是一行字符串 use strict。
‘use strict’;
老版本的引擎会把它当作一行普通字符串，加以忽略。新版本的引擎就会进入严格模式。
严格模式可以用于整个脚本，也可以只用于单个函数

1. 整个脚本文件
use strict 放在脚本文件的第一行，整个脚本都将以严格模式运行。如果这行语句不在第一行就无效，整个脚本会以正常模式运行。(严格地说，只要前面不是产生实际运行结果的语句，use strict 可以不在第一行，比如直接跟在一个空的分号后面，或者跟在注释后面。)

```js
<script>'use strict'; console.log('这是严格模式');</script>
```

```js
<script>console.log('这是正常模式');</script>
```

上面代码中，一个网页文件依次有两段 JavaScript 代码。前一个 script 标签是严格模式，后一个不是。
如果 use strict 写成下面这样，则不起作用，严格模式必须从代码一开始就生效。

```js
<script>console.log('这是正常模式'); 'use strict';</script>
```

2. 单个函数
use strict 放在函数体的第一行，则整个函数以严格模式运行。

```js
function strict() {
  "use strict";
  return "这是严格模式";
}
function notStrict() {
  return "这是正常模式";
}
```

### 严格模式下的要求

变量声明（必须要使用 var）
函数不能有重名的参数
正常模式下，如果函数有多个重名的参数，可以用 arguments[i]读取。严格模式下，这属于语法错误。

```js
function f(a, a, b) {
  "use strict";
  return a + b;
}
```

禁止八进制的前缀 0 表示法
正常模式下，整数的第一位如果是 0，表示这是八进制数，比如 0100 等于十进制的 64。严格模式禁止这种表示法，整数第一位为 0，将报错。

```js
"use strict";
var n = 0100;
```

禁止 this 关键字指向全局对象
正常模式下，函数内部的 this 可能会指向全局对象（window），严格模式禁止这种用法，避免无意间创造全局变量。

```js
function f() {
  // 正常模式
  console.log(this === window);
}
f(); // true
function f() {
  // 严格模式
  "use strict";
  console.log(this === undefined);
}
f(); // true
上面代码中，严格模式的函数体内部 this 是 undefined。这种限制对于构造函数尤其有用。使用构造函数时，有时忘了加 new，这时 this 不再指向全局对象，而是报错。
```

```js
function f() {
  "use strict";
  this.a = 1;
}
f(); // 报错，this 未定义
```

严格模式下，使用 with 语句将报错。

```js
"use strict";
var obj = { v: 1 };
with (obj) {
  v = 2; //obj.v = 2;
}
```

### 创设 eval 作用域

正常模式下，JavaScript 语言有两种变量作用域（scope）：全局作用域和函数作用域。严格模式创设了第三种作用域：eval 作用域。

正常模式下，eval 语句的作用域，取决于它处于全局作用域，还是函数作用域。严格模式下，eval 语句本身就是一个作用域，不再能够在其所运行的作用域创设新的变量了，也就是说，eval 所生成的变量只能用于 eval 内部。

```js
(function () {
  "use strict";
  var x = 2;
  console.log(eval("var x = 5; x")); // 5
  console.log(x); // 2
})();
```

上面代码中，由于 eval 语句内部是一个独立作用域，所以内部的变量 x 不会泄露到外部

### ES5 新增的数组方法

### 静态方法

Array.isArray()
Array.isArray 方法返回一个布尔值，表示参数是否为数组。它可以弥补 typeof 运算符的不足。

```js
var arr = [1, 2, 3];
typeof arr; // "object"
Array.isArray(arr); // true
```

上面代码中，typeof 运算符只能显示数组的类型是 Object，而 Array.isArray 方法可以识别数组。

### 实例（对象）方法

1. map方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回。

```js
var numbers = [1, 2, 3];
var res = numbers.map(function (n) {
  return n + 1;
});
res; // [2, 3, 4]
numbers;
// [1, 2, 3]
```

上面代码中，numbers 数组的所有成员依次执行参数函数，运行结果组成一个新数组返回，原数组没有变化。
map 方法接受一个函数作为参数。该函数调用时，map 方法向它传入三个参数：当前成员、当前位置和数组本身。

```js
[1, 2, 3].map(function (elem, index, arr) {
  return elem * index;
});
// [0, 2, 6]
```

上面代码中，map 方法的回调函数有三个参数，elem 为当前成员的值，index 为当前成员的位置，arr 为原数组（[1, 2, 3]）

2. forEach方法与 map 方法很相似，也是对数组的所有成员依次执行参数函数。但是，forEach 方法不返回值，只用来操作数据。这就是说，如果数组遍历的目的是为了得到返回值，那么使用 map 方法，否则使用 forEach 方法。
forEach 的用法与 map 方法一致，参数是一个函数，该函数同样接受三个参数：当前值、当前位置、整个数组。

```js
function log(element, index, array) {
  console.log("[" + index + "] = " + element);
}
[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```

注意，forEach 方法无法中断执行，总是会将所有成员遍历完。如果希望符合某种条件时，就中断遍历，要使用 for 循环。

```js
var arr = [1, 2, 3];
for (var i = 0; i < arr.length; i++) {
  if (arr[i] === 2) break;
  console.log(arr[i]);
}
```

3. filter方法用于过滤数组成员，满足条件的成员组成一个新数组返回。
   它的参数是一个函数，所有数组成员依次执行该函数，返回结果为 true 的成员组成一个新数组返回。该方法不会改变原数组。
   filter 方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组。

```js
var res = [1, 2, 3, 4, 5].filter(function (elem, index, arr) {
  return index % 2 === 0;
});
res; // [1, 3, 5]
```

上面代码返回偶数位置的成员组成的新数组。

4. reduce方法依次处理数组的每个成员，最终累计为一个值。reduce 是从左到右处理（从第一个成员到最后一个成员）
reduce 方法参数是一个函数,该函数接受以下两个参数。
   1. 累积变量，默认为数组的第一个成员
   2. 当前变量，默认为数组的第二个成员

```js
var res = [1, 2, 3, 4, 5].reduce(function (a, b) {
  console.log(a, b);
  return a + b;
});
// 1 2
// 3 3
// 6 4
// 10 5
res; //最后结果：15
```

上面代码中，reduce 方法求出数组所有成员的和。第一次执行，a 是数组的第一个成员 1，b 是数组的第二个成员 2。第二次执行，a 为上一轮的返回值 3，b 为第三个成员 3。第三次执行，a 为上一轮的返回值 6，b 为第四个成员 4。第四次执行，a 为上一轮返回值 10，b 为第五个成员 5。至此所有成员遍历完成，整个方法的返回值就是最后一轮的返回值 15。

5. indexOf(),lastIndexOf()
   indexOf 方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。

```js
var a = ["a", "b", "c"];
a.indexOf("b"); // 1
a.indexOf("y") // -1
  [
    //indexOf方法还可以接受第二个参数，表示搜索的开始位置。
    ("a", "b", "c")
  ].indexOf("a", 1); // -1
//上面代码从1号位置开始搜索字符a，结果为-1，表示没有搜索到。
```

lastIndexOf 方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。

```js
var a = [2, 5, 9, 2];
a.lastIndexOf(2); // 3
a.lastIndexOf(7); // -1
```

注意，这两个方法不能用来搜索 NaN 的位置，即它们无法确定数组成员是否包含 NaN。

```js
[NaN]
  .indexOf(NaN) // -1
  [NaN].lastIndexOf(NaN); // -1
```

这是因为这两个方法内部，使用严格相等运算符（===）进行比较，
而 NaN 是唯一一个不等于自身的值

### 字符串 string

### 字符串的创建方式

```js
var s1 = "abc";
var s2 = new String("abc");
typeof s1; // "string"
typeof s2; // "object"
```

上面代码中，变量 s1 是字符串，s2 是对象。
所以,String 对象也叫包装对象
除了用作构造函数，String 对象还可以当作工具方法使用，将任意类型的值转为字符串。

```js
String(true); // "true"
String(5); // "5"
```

上面代码将布尔值 ture 和数值 5，分别转换为字符串

字符串实例的 length 属性返回字符串的长度。

```js
"abc".length; // 3
```

字符串对象是一个类似数组的对象（很像数组，但不是数组）。

```js
new String("abc")(
  // String {0: "a", 1: "b", 2: "c", length: 3}
  new String("abc")
)[1]; // "b"
```

上面代码中，字符串 abc 对应的字符串对象，有数值键（0、1、2）和 length 属性，所以可以像数组那样取值。

### charAt 方法
charAt 方法返回指定位置的字符，参数是从 0 开始编号的位置。

```js
var s = new String("abc");
s.charAt(1); // "b"
s.charAt(s.length - 1); // "c"
//这个方法完全可以用数组下标替代。
"abc".charAt(1); // "b"
"abc"[1]; // "b"
```

### slice 方法
slice 方法用于从原字符串取出子字符串并返回，不改变原字符串。它的第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含该位置）。

```js
"JavaScript".slice(0, 4); // "Java"
//如果省略第二个参数，则表示子字符串一直到原字符串结束。
"JavaScript".slice(4); // "Script"
//如果参数是负值，表示从结尾开始倒数计算的位置，即该负值加上字符串长度。
"JavaScript".slice(-6); // "Script"
"JavaScript".slice(0, -6); // "Java"
"JavaScript".slice(-2, -1); // "p"
//如果第一个参数大于第二个参数，slice方法返回一个空字符串。
"JavaScript".slice(2, 1); //
```

### substring 方法
substring 方法用于从原字符串取出子字符串并返回，不改变原字符串，跟 slice 方法很相像。它的第一个参数表示子字符串的开始位置，第二个位置表示结束位置（返回结果不含该位置）。

```js
"JavaScript".substring(0, 4); // "Java"
//如果省略第二个参数，则表示子字符串一直到原字符串的结束。
"JavaScript".substring(4); // "Script"
//如果第二个参数大于第一个参数，substring方法会自动更换两个参数的位置。
"JavaScript".substring(10, 4); // "Script"
// 等同于
"JavaScript".substring(4, 10); // "Script"
```

### substr 方法
substr 方法用于从原字符串取出子字符串并返回，不改变原字符串，跟 slice 和 substring 方法的作用相同。
substr 方法的第一个参数是子字符串的开始位置（从 0 开始计算），第二个参数是子字符串的长度。

```js
"JavaScript".substr(4, 6); // "Script"
//如果省略第二个参数，则表示子字符串一直到原字符串的结束。
"JavaScript".substr(4); // "Script"
//如果第一个参数是负数，表示倒数计算的字符位置。如果第二个参数是负数，将被自动转为0，因此会返回空字符串。
"JavaScript".substr(-6); // "Script"
"JavaScript".substr(4, -1); // ""
```

//上面代码中，第二个例子的参数-1自动转为0，表示子字符串长度为0，所以返回空字符串

### indexOf 方法
indexOf 方法用于确定一个字符串在另一个字符串中第一次出现的位置，返回结果是匹配开始的位置。如果返回-1，就表示不匹配。

```js
"hello world".indexOf("o"); // 4
"JavaScript".indexOf("script"); // -1
//indexOf方法还可以接受第二个参数，表示从该位置开始向后匹配。
"hello world".indexOf("o", 6); // 7
//lastIndexOf方法的用法跟indexOf方法一致，主要的区别是lastIndexOf从尾部开始匹配，indexOf则是从头部开始匹配。
"hello world".lastIndexOf("o"); // 7
//另外，lastIndexOf的第二个参数表示从该位置起向前匹配。
"hello world".lastIndexOf("o", 6); // 4
```

### trim 方法

trim 方法用于去除字符串两端的空格，返回一个新字符串，不改变原字符串。

```js
"hello world".trim();
// "hello world"
//该方法去除的不仅是空格，还包括制表符（\t、\v）、换行符（\n）和回车符（\r）。
"\r\nabc \t".trim(); // 'abc'
```

### toLowerCase 方法

toLowerCase 方法用于将一个字符串全部转为小写，toUpperCase 则是全部转为大写。它们都返回一个新字符串，不改变原字符串。

```js
"Hello World".toLowerCase();
// "hello world"
"Hello World".toUpperCase();
// "HELLO WORLD"
```

### replace 方法

replace 方法用于替换匹配的子字符串，一般情况下只替换第一个匹配（除非使用带有 g 修饰符的正则表达式）。

```js
"aaa".replace("a", "b"); // "baa"
"aaa".replace(/a/g, "b"); // "bbb"
```

### split 方法

split 方法按照给定规则分割字符串，返回一个由分割出来的子字符串组成的数组。

```js
"a|b|c".split("|"); // ["a", "b", "c"]
//如果分割规则为空字符串，则返回数组的成员是原字符串的每一个字符。
"a|b|c".split(""); // ["a", "|", "b", "|", "c"]
```

### ASCII 码和字符集

字符串常见 API(charCodeAt\fromCharCode)
charCodeAt方法返回字符串指定位置的 Unicode 码点（十进制表示），相当于String.fromCharCode()的逆操作。
'abc'.charCodeAt(1) // 98
上面代码中，abc 的 1 号位置的字符是 b，它的 Unicode 码点是 98。
String.fromCharCode()
String 对象提供的静态方法（即定义在对象本身，而不是定义在对象实例的方法），主要是String.fromCharCode()。该方法的参数是一个或多个数值，代表 Unicode 码点，返回值是这些码点组成的字符串。

```js
String.fromCharCode(); // ""
String.fromCharCode(97); // "a"
String.fromCharCode(104, 101, 108, 108, 111);
// "hello"
```

统计字符串中每个字符的个数

---

本文作者： 一只野生东子

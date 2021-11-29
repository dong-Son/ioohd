---
title: ES6
date: 2020-02-08
tags: javaScript、es6
---

ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在 2015 年 6 月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

<!--more--> 

### this 关键字

this 可以用在构造函数之中，表示实例对象。除此之外，this 还可以用在别的场合。但不管是什么场合，this 都有一个共同点：它总是返回一个对象
简单说，this 就是属性或方法“当前”所在的对象。

```js
var person = {
  name: "张三",
  describe: function () {
    return "姓名：" + this.name;
  },
};
person.describe(); // "姓名：张三"
```

上面代码中，this.name表示 name 属性所在的那个对象。由于this.name是在 describe 方法中调用，而 describe 方法所在的当前对象是 person，因此 this 指向 person，this.name就是person.name。

### this 主要有以下几个使用场合

1. 全局环境
   全局环境使用 this，它指的就是顶层对象 window。

```js
this === window; // true
function f() {
  console.log(this === window);
}
f(); // true
//注：严格模式下 普通函数内部 this 等于 undefined
```

2. 构造函数
   构造函数中的 this，指的是实例对象

```js
function Person(p) {
  this.p = p;
}
var obj = new Person();
```

3. 对象的方法
   如果对象的方法里面包含 this，this 的指向就是方法运行时所在的对象。

```js
var obj = {
  foo: function () {
    console.log(this);
  },
};
obj.foo(); // obj
```

### bind 方法

bind 方法用于将函数体内的 this 绑定到某个对象，然后返回一个新函数。

```js
var dog = {
  name: "wangcai",
  age: 18,
  wang: function () {
    console.log(this);
  },
};
var person = { name: "小明" };
var func = dog.wang.bind(person);
func();
//上面代码将person绑定到了func函数内部
``` 

### let/const

1. ES6 新增了 let 命令，用来声明变量。它的用法类似于 var，但是所声明的变量，只在 let 命令所在的代码块内有效。

```js
{
  let a = 10;
  var b = 1;
}
a; // ReferenceError: a is not defined.
b; // 1
```

上面代码在代码块之中，分别用 let 和 var 声明了两个变量。然后在代码块之外调用这两个变量，结果 let 声明的变量报错，var 声明的变量返回了正确的值。这表明，let 声明的变量只在它所在的代码块有效。

for 循环的计数器，就很合适使用 let 命令。

```js
var oLis = document.getElementsByTagName("li");
for (let i = 0; i < oLis.length; i++) {
  // ...
  oLis[i].onclick = function () {
    console.log(i); // 0 1 2 3 4
  };
}
console.log(i);
// ReferenceError: i is not defined
//上面代码相当与产生了几个块级作用域
{
  var i = 0;
  oLis[i].onclick = function () {
    console.log(i);
  };
}
{
  var i = 1;
  oLis[i].onclick = function () {
    console.log(i);
  };
}
```

上面代码中，计数器 i 只在 for 循环体内有效，在循环体外引用就会报错。

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景，let 实际上为 JavaScript 新增了块级作用域。

var 命令会发生”变量提升“现象，即变量可以在声明之前使用，值为 undefined。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。

为了纠正这种现象，let 命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

2. const 命令
   const 声明一个只读的常量。一旦声明，常量的值就不能改变。
   const 声明的变量不得改变值，这意味着，const 一旦声明变量，就必须立即初始化，不能留到以后赋值

```js
const PI = 3.1415;
PI; // 3.1415
PI = 3;
// TypeError: Assignment to constant variable.
```

上面代码表明改变常量的值会报错。

### 箭头函数

ES6 允许使用“箭头”（=>）定义函数。

```js
var f = (v) => v;
// 等同于
var f = function (v) {
  return v;
};
```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。

```js
var f = () => 5;
// 等同于
var f = function () {
  return 5;
};
var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function (num1, num2) {
  return num1 + num2;
};
```

如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用 return 语句返回。

```js
var sum = (num1, num2) => {
  return num1 + num2;
};
```

箭头函数使得表达更加简洁。

```js
const isEven = (n) => n % 2 == 0;
const square = (n) => n * n;
```

上面代码只用了两行，就定义了两个简单的工具函数。如果不用箭头函数，可能就要占用多行，而且还不如现在这样写醒目。

箭头函数的一个用处是简化回调函数

```js
// 正常函数写法
[1, 2, 3].map(function (x) {
  return x * x;
});
// 箭头函数写法
[1, 2, 3].map((x) => x * x);

// 正常函数写法
var result = values.sort(function (a, b) {
  return a - b;
});
// 箭头函数写法
var result = values.sort((a, b) => a - b);
```

箭头函数有几个使用注意点。

1. 函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。
2. 不可以当作构造函数，也就是说，不可以使用 new 命令，否则会抛出一个错误。
3. 不可以使用 arguments 对象，该对象在函数体内不存在。

### 变量解构（解构赋值）

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构
以前，为变量赋值，只能直接指定值。

```js
let a = 1;
let b = 2;
let c = 3;
```

ES6 允许写成下面这样。

```js
let [a, b, c] = [1, 2, 3];
```

本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值
解构不仅可以用于数组，还可以用于对象

```js
let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo; // "aaa"
bar; // "bbb"
```

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值

```js
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo; // "aaa"
bar; // "bbb"
let { foo } = { foo: "aaa", bar: "bbb" };
foo; // "aaa"
```

### 字符串扩展

模板字符串（template string）是增强版的字符串

传统的 JavaScript 语言，输出模板通常是这样写的。

```js
var a = 1;
var b = 2;
var sum = a + b;
var res = a + "+" + b + "的和是<b>" + c + "</b>";
console.log(res);
box.innerHTML = res;
var obj = { name: "zhangsan", age: 18 };
var str = "<b>姓名</b>：" + obj.name + "<b>年龄</b>:" + obj.age;
div.innerHTML = str;
```

上面这种写法(拼字符串)相当繁琐不方便，ES6 引入了模板字符串解决这个问题。
模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，可以在字符串中嵌入变量， 模板字符串中嵌入变量，需要将变量名写在${}之中。

```js
var a = 1;
var b = 2;
var sum = a + b;
var res = `a+b的和是<b>${c}</b>`;
console.log(res);
box.innerHTML = res;
var obj = { name: "zhangsan", age: 18 };
var str = `<b>姓名</b>：${obj.name}<b>年龄</b>:${obj.age}`;
div.innerHTML = str;
```
上面代码中的模板字符串，都是用反引号表示。如果在模板字符串中需要使用反引号，则前面要用反斜杠转义

```js
let greeting = `\`Yo\` World!`;
```

如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中

```js
oDiv.innerHTML = `
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`;
```

大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。

```js
et x = 1;let y = 2;

`${x} + ${y} = ${x + y}`
// "1 + 2 = 3"

`${x} + ${y * 2} = ${x + y * 2}`
// "1 + 4 = 5"
let obj = {x: 1, y: 2};
`${obj.x + obj.y}`
```

模板字符串之中还能调用函数

```js
function fn() {
  return "Hello World";
}

`foo ${fn()} bar`;
// foo Hello World bar
```

字符串扩张方法
传统上，JavaScript 只有 indexOf 方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。
includes()：返回布尔值，表示是否找到了参数字符串。
startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。

```js
let s = "Hello world!";

s.startsWith("Hello"); // true
s.endsWith("!"); // true
s.includes("o"); // true
``` 

repeat 方法返回一个新字符串，表示将原字符串重复 n 次

```js
"x".repeat(3); // "xxx"
"hello".repeat(2); // "hellohello"
"na".repeat(0); // ""
```

### 数组新增方法

Array.from 方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（ES6 新增的数据结构 Set 和 Map）
下面是一个类似数组的对象，Array.from 将它转为真正的数组

```js
let arrayLike = {
  0: "a",
  1: "b",
  2: "c",
  length: 3,
};
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
``` 

实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的 arguments 对象。Array.from 都可以将它们转为真正的数组

### 对象扩展

ES6 允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```js
const foo = "bar";
const baz = { foo };
baz; // {foo: "bar"}
// 等同于
const baz = { foo: foo };
```

除了属性简写，方法也可以简写。

```js
const o = {
  method() {
    return "Hello!";
  },
};
// 等同于
const o = {
  method: function () {
    return "Hello!";
  },
};
```

### 扩展运算符

对象的扩展运算符（…）用于取出参数对象的所有可遍历属性，拷贝到当前对象之中。

```js
let z = { a: 3, b: 4 };
let n = { ...z };
n; // { a: 3, b: 4 }
```

由于数组是特殊的对象，所以对象的扩展运算符也可以用于数组。

```js
let foo = { ...["a", "b", "c"] };
foo;
// {0: "a", 1: "b", 2: "c"}
```

对象的扩展运算符等同于使用Object.assign()方法。

```js
let aClone = { ...a };
// 等同于
let aClone = Object.assign({}, a);
```

```js
function add(x, y) {
  return x + y;
}
var numbers = [4, 38];
add(...numbers); //
//该运算符将一个数组，变为参数序列
```

```js
var arr1 = ["a", "b"];
var arr2 = ["c"];
var arr3 = ["d", "e"];
// 合并数组
[...arr1, ...arr2, ...arr3];
```

### 函数参数默认值

ES6 之前，不能直接为函数的参数指定默认值，只能采用变通的方法。

```js
function log(x, y) {
  y = y || "World";
  console.log(x, y);
}
log("Hello"); // Hello World
log("Hello", "China"); // Hello China
//这种写法的缺点在于，如果参数y赋值了，但是对应的布尔值为false，则该赋值不起作用。
```

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

```js
function log(x, y = "World") {
  console.log(x, y);
}
log("Hello"); // Hello World
log("Hello", "China"); // Hello China
```

可以看到，ES6 的写法比 ES5 简洁许多，而且非常自然。

### Symbol 类型

ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法，新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入 Symbol 的原因

ES6 引入了一种新的原始数据类型 Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）

Symbol 值通过 Symbol 函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突

```js
let s = Symbol();
typeof s;
// "symbol"
```

上面代码中，变量 s 就是一个独一无二的值。typeof 运算符的结果，表明变量 s 是 Symbol 数据类型，而不是字符串之类的其他类型
注意，Symbol 函数前不能使用 new 命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。
Symbol 函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```js
let s1 = Symbol("foo");
let s2 = Symbol("bar");
s1; // Symbol(foo)
s2; // Symbol(bar)
s1.toString(); // "Symbol(foo)"
s2.toString(); // "Symbol(bar)"
```

上面代码中，s1 和 s2 是两个 Symbol 值。如果不加参数，它们在控制台的输出都是 Symbol()，不利于区分。有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。

注意，Symbol 函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的 Symbol 函数的返回值是不相等的。

```js
// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();
s1 === s2; // false
// 有参数的情况
let s1 = Symbol("foo");
let s2 = Symbol("foo");
s1 === s2; // false
```

上面代码中，s1 和 s2 都是 Symbol 函数的返回值，而且参数相同，但是它们是不相等的。
Symbol 值也可以转为布尔值，但是不能转为数值

```js
let sym = Symbol();
Boolean(sym); // true
!sym; // false
if (sym) {
  // ...
}

Number(sym); // TypeError
sym + 2; // TypeError
```

作为属性名的 Symbol
由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性，能防止某一个键被不小心改写或覆盖。

```js
// 第一种写法
let a = {};
a[mySymbol] = "Hello!";
// 第二种写法
let a = {
  [mySymbol]: "Hello!",
};
// 以上写法都得到同样结果
a[mySymbol]; // "Hello!"
```

注意，Symbol 值作为对象属性名时，不能用点运算符。

```js
const mySymbol = Symbol();
const a = {};

a.mySymbol = "Hello!";
a[mySymbol]; // undefined
a["mySymbol"]; // "Hello!"
```

上面代码中，因为点运算符后面总是字符串，所以不会读取 mySymbol 作为标识名所指代的那个值，导致 a 的属性名实际上是一个字符串，而不是一个 Symbol 值。
同理，在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。

```js
let s = Symbol();

let obj = {
  [s]: function (arg) { ... }};

obj[s](123);
```

上面代码中，如果 s 不放在方括号中，该属性的键名就是字符串 s，而不是 s 所代表的那个 Symbol 值。

### Set 和 Map 结构

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
Set 本身是一个构造函数，用来生成 Set 数据结构。

```js
const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach((x) => s.add(x));
for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

上面代码通过 add 方法向 Set 结构加入成员，结果表明 Set 结构不会添加重复的值。
Set 函数可以接受一个数组（获取 dom 的 nodelist 对象）作为参数，用来初始化。

```js
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set];
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size; // 5

// 例三
const set = new Set(document.querySelectorAll("div"));
set.size; // 56
```

上面代码也展示了一种去除数组重复成员的方法。

```js
// 去除数组的重复成员
[...new Set(array)];
```

向 Set 加入值的时候，不会发生类型转换，所以 5 和”5”是两个不同的值。Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（===），主要的区别是 NaN 等于自身，而精确相等运算符认为 NaN 不等于自身。

```js
let set = new Set();
let a = NaN;
let b = NaN;
set.add(a);
set.add(b);
set; // Set {NaN}
```

Set 结构的实例有以下属性
constructor：构造函数，默认就是 Set 函数。
size：返回 Set 实例的成员总数。
Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面四个操作方法

```js
//add(value):添加某个值，返回 Set 结构本身。
//delete(value):删除某个值，返回一个布尔值，表示删除是否成功。
//has(value)：返回一个布尔值，表示该值是否为Set的成员。
//clear()：清除所有成员，没有返回值。
s.add(1).add(2).add(2);
// 注意2被加入了两次
s.size; // 2
s.has(1); // true
s.has(2); // true
s.has(3); // false
s.delete(2);
s.has(2); // false
```

### Array.from 方法可以将 Set 结构转为数组。

```js
let set = new Set(["red", "green", "blue"]);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```

Set 结构的实例与数组一样，也拥有forEach方法，用于对每个成员执行某种操作，没有返回值。

```js
set = new Set([1, 4, 9]);
set.forEach((value, key) => console.log(key + " : " + value));
// 1 : 1
// 4 : 4
// 9 : 9
```

上面代码说明，forEach 方法的参数就是一个处理函数。该函数的参数与数组的 forEach 一致，依次为键值、键名、集合本身（上例省略了该参数）。这里需要注意，Set 结构的键名就是键值（两者是同一个值），因此第一个参数与第二个参数的值永远都是一样的。

扩展运算符和 Set 结构相结合，就可以去除数组的重复成员。

```js
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]
```

map 结构
JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

Map 结构的实例有以下属性和操作方法。

1. size 属性
   size 属性返回 Map 结构的成员总数。

```js
const map = new Map();
map.set("foo", true);
map.set("bar", false);

map.size; // 2
```

2. set(key, value)
   set 方法设置键名 key 对应的键值为 value，然后返回整个 Map 结构。如果 key 已经有值，则键值会被更新，否则就新生成该键。

```js
const m = new Map();
m.set("edition", 6); // 键是字符串
m.set(262, "standard"); // 键是数值
m.set(undefined, "nah"); // 键是 undefined
```

set 方法返回的是当前的 Map 对象，因此可以采用链式写法。

```js
let map = new Map().set(1, "a").set(2, "b").set(3, "c");
```

3. get(key)
   get 方法读取 key 对应的键值，如果找不到 key，返回 undefined。

```js
const m = new Map();

const hello = function () {
  console.log("hello");
};
m.set(hello, "Hello ES6!"); // 键是函数

m.get(hello); // Hello ES6!
```

4. has(key)
   has 方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

```js
const m = new Map();

m.set("edition", 6);
m.set(262, "standard");
m.set(undefined, "nah");

m.has("edition"); // true
m.has("years"); // false
m.has(262); // true
m.has(undefined); // true
``` 

5. delete(key)
   delete 方法删除某个键，返回 true。如果删除失败，返回 false。

```js
const m = new Map();
m.set(undefined, "nah");
m.has(undefined); // true

m.delete(undefined);
m.has(undefined); // false
``` 

6. clear()
   clear 方法清除所有成员，没有返回值。

```js
let map = new Map();
map.set("foo", true);
map.set("bar", false);
map.size; // 2
map.clear();
map.size; // 0
``` 

遍历 map

需要特别注意的是，Map 的遍历顺序就是插入顺序

```js
const map = new Map([
  ["F", "no"],
  ["T", "yes"],
]);
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
``` 

### Generators 生成器函数

Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同
Generator 函数有多种理解角度。语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。
执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function 关键字与函数名之间有一个星号；二是，函数体内部使用 yield 表达式，定义不同的内部状态（yield 在英语里的意思就是“产出”）

```js
function* helloWorldGenerator() {
  yield "hello";
  yield "world";
  return "ending";
}
var hw = helloWorldGenerator();
``` 

上面代码定义了一个 Generator 函数 helloWorldGenerator，它内部有两个 yield 表达式（hello 和 world），即该函数有三个状态：hello，world 和 return 语句（结束执行）。

然后，Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象

### yield 表达式

由于 Generator 函数返回的遍历器对象，只有调用 next 方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。yield 表达式就是暂停标志。

1. 遇到 yield 表达式，就暂停执行后面的操作，并将紧跟在 yield 后面的那个表达式的值，作为返回的对象的 value 属性值。
2. 下一次调用 next 方法时，再继续往下执行，直到遇到下一个 yield 表达式。
3. 如果没有再遇到新的 yield 表达式，就一直运行到函数结束，直到 return 语句为止，并将 return 语句后面的表达式的值，作为返回的对象的 value 属性值。
4. 如果该函数没有 return 语句，则返回的对象的 value 属性值为 undefined

下一步，必须调用遍历器对象的 next 方法，使得指针移向下一个状态。也就是说，每次调用 next 方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个 yield 表达式（或 return 语句）为止。换言之，Generator 函数是分段执行的，yield 表达式是暂停执行的标记，而 next 方法可以恢复执行。

```js 
hw.next();
// { value: 'hello', done: false }
hw.next();
// { value: 'world', done: false }
hw.next();
// { value: 'ending', done: true }
hw.next();
// { value: undefined, done: true }
```

上面代码一共调用了四次 next 方法。
第一次调用，Generator 函数开始执行，直到遇到第一个 yield 表达式为止。next 方法返回一个对象，它的 value 属性就是当前 yield 表达式的值 hello，done 属性的值 false，表示遍历还没有结束。
第二次调用，Generator 函数从上次 yield 表达式停下的地方，一直执行到下一个 yield 表达式。next 方法返回的对象的 value 属性就是当前 yield 表达式的值 world，done 属性的值 false，表示遍历还没有结束。
第三次调用，Generator 函数从上次 yield 表达式停下的地方，一直执行到 return 语句（如果没有 return 语句，就执行到函数结束）。next 方法返回的对象的 value 属性，就是紧跟在 return 语句后面的表达式的值（如果没有 return 语句，则 value 属性的值为 undefined），done 属性的值 true，表示遍历已经结束。
第四次调用，此时 Generator 函数已经运行完毕，next 方法返回对象的 value 属性为 undefined，done 属性为 true。以后再调用 next 方法，返回的都是这个值。

总结一下，调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的 next 方法，就会返回一个有着 value 和 done 两个属性的对象。value 属性表示当前的内部状态的值，是 yield 表达式后面那个表达式的值；done 属性是一个布尔值，表示是否遍历结束。

### class 的写法及继承

JavaScript 语言中，生成实例对象的传统方法是通过构造函数。下面是一个例子

```js
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return "(" + this.x + ", " + this.y + ")";
};

var p = new Point(1, 2);
``` 

上面这种写法跟传统的面向对象语言（比如 C++ 和 Java）差异很大，很容易让新学习这门语言的程序员感到困惑。
ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类。

基本上，ES6 的 class 可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用 ES6 的 class 改写，就是下面这样。

```js
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return "(" + this.x + ", " + this.y + ")";
  }
}
``` 

上面代码定义了一个“类”，可以看到里面有一个 constructor 方法，这就是构造方法，而 this 关键字则代表实例对象。也就是说，ES5 的构造函数 Point，对应 ES6 的 Point 类的构造方法

point 类除了构造方法，还定义了一个 toString 方法。注意，定义“类”的方法的时候，前面不需要加上 function 这个关键字，直接把函数定义放进去了就可以了。另外，方法之间不需要逗号分隔，加了会报错。

```js
//hasOwnProperty 可以用来判断对象是否有每一个属性
point.hasOwnProperty("x"); // true
point.hasOwnProperty("y"); // true
```

ES6 的类，完全可以看作构造函数的另一种写法

```js
class Point {
  // ...
}

typeof Point; // "function"
```

上面代码表明，类的数据类型就是函数，类本身就指向构造函数

类的属性名，可以采用表达式。

```js
let methodName = "getArea";

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
``` 

上面代码中，Square 类的方法名 getArea，是从表达式得到的。

类内部，默认就是严格模式，所以不需要使用 use strict 指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。

考虑到未来所有的代码，其实都是运行在模块之中，所以 ES6 实际上把整个语言升级到了严格模式。

### constructor 方法

constructor 方法是类的默认方法，通过 new 命令生成对象实例时，自动调用该方法。一个类必须有 constructor 方法，如果没有显式定义，一个空的 constructor 方法会被默认添加。

```js
class Point {}
// 等同于
class Point {
  constructor() {}
}
```

上面代码中，定义了一个空的类 Point，JavaScript 引擎会自动为它添加一个空的 constructor 方法。

constructor 方法默认返回实例对象（即 this）

类必须使用 new 调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用 new 也可以执行。

```js
class Foo {
  constructor() {}
}

Foo();
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

生成类的实例对象的写法，与 ES5 完全一样，也是使用 new 命令。前面说过，如果忘记加上 new，像函数那样调用 Class，将会报错。

```js
class Point {
  // ...
}
// 报错
var point = Point(2, 3);
// 正确
var point = new Point(2, 3);
```

类不存在变量提升（hoist），这一点与 ES5 完全不同。

```js
new Foo(); // ReferenceError
class Foo {}
```

类方法
加上 static 关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
class Foo {
  static classMethod() {
    return "hello";
  }
}

Foo.classMethod(); // 'hello'
var foo = new Foo();
foo.classMethod();
// TypeError: foo.classMethod is not a function
```

上面代码中，Foo 类的 classMethod 方法前有 static 关键字，表明该方法是一个静态方法，可以直接在 Foo 类上调用（Foo.classMethod()），而不是在 Foo 类的实例上调用。如果在实例上调用静态方法，会抛出一个错误，表示不存在该方法。

### 类的继承

Class 可以通过 extends 关键字实现继承 这比 ES5 的通过修改原型链（在后面章节会讲解）实现继承，要清晰和方便很多。

```js
class Point {}

class ColorPoint extends Point {}
```

上面代码定义了一个 ColorPoint 类，该类通过 extends 关键字，继承了 Point 类的所有属性和方法。但是由于没有部署任何代码，所以这两个类完全一样，等于复制了一个 Point 类。下面，我们在 ColorPoint 内部加上代码。

```js
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + " " + super.toString(); // 调用父类的toString()
  }
}
```

上面代码中，constructor 方法和 toString 方法之中，都出现了 super 关键字，它在这里表示父类的构造函数，用来新建父类的 this 对象。

ES6 要求，子类的构造函数必须执行一次 super 函数

子类必须在 constructor 方法中调用 super 方法，否则新建实例时会报错。这是因为子类自己的 this 对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用 super 方法，子类就得不到 this 对象。

```js
class Point {
  /* ... */
}

class ColorPoint extends Point {
  constructor() {}
}

let cp = new ColorPoint(); // ReferenceError
```

上面代码中，ColorPoint 继承了父类 Point，但是它的构造函数没有调用 super 方法，导致新建实例时报错。

需要注意的地方是，在子类的构造函数中，只有调用 super 之后，才可以使用 this 关键字，否则会报错。这是因为子类实例的构建，是基于对父类实例加工，只有 super 方法才能返回父类实例

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```

上面代码中，子类的 constructor 方法没有调用 super 之前，就使用 this 关键字，结果报错，而放在 super 方法之后就是正确的。

--- 

本文作者： 一只野生东子
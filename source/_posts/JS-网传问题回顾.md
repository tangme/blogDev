---
title: JS-网传问题回顾
date: 2018-06-07 11:41:31
index_img: https://s3.ax1x.com/2021/03/10/6JdNNQ.png
tags: [javascript]
---

# 问题

- 使用 typeof bar ===“object”来确定 bar 是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？
- let a = b = c = 'value'; 方式声明赋值时，有没有潜在风险？

# 答案

## 使用 typeof bar === "object" 来确定 bar 是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？

之所以使用 `typeof bar === object"` ，是因为大多数情况下我们想检测一个变量是否为一个对象。但是当待检测的变量为`null` ，或者赋值方式为以下几种方式时，如果没有深入了解数据类型的话，则会有挠头的姿态出现了：

```javascript
let a = new Date();
let b = new Number(110);
let c = new String("hello world");
let d = new Boolean(true);
let e = "just string";

console.log(typeof null);
console.log(typeof a, a, a.toString());
console.log(typeof b, b, b.toString());
console.log(typeof c, c, c.toString());
console.log(typeof d, d, d.toString());
console.log(typeof e);
```

这里当值为 `null`，或者以 `new`关键字形式赋值后，输出的值均为 object。这里不深究数据类型，但如果我们想确切的检测一个变量的类型时，可以参考 jQuery 的实现方式：

```javascript
/*此处为非jQuery，但参考jQuery而来的实现方式代码块*/
const class2type = {};
"Boolean Number String Function Array Date RegExp Object Error Symbol".split(" ").map(function (item) {
	class2type["[object " + item + "]"] = item.toLowerCase();
});
function type(obj) {
	if (obj == null) {
		return obj + "";
	}

	// Support: Android <=2.3 only (functionish RegExp)
	return typeof obj === "object" || typeof obj === "function" ? class2type[toString.call(obj)] || "object" : typeof obj;
}

let a = new Date();
let b = new Number(110);
let c = new String("hello world");
let d = new Boolean(true);
let e = "just string";

console.log(type(null));
console.log(type(a));
console.log(type(b));
console.log(type(c));
console.log(type(d));
console.log(type("just string"));
```

## let a = b = c = 'value'; 方式声明赋值时，有没有潜在风险？

```javascript
(function () {
	var a = (b = c = 3);
})();

console.log("a defined? " + (typeof a !== "undefined"), typeof a);
console.log("b defined? " + (typeof b !== "undefined"), typeof b);
console.log("c defined? " + (typeof c !== "undefined"), typeof c);
```

会打印出什么内容呢？想一想。

打印的结果是：a 为 undefined，b 和 c 为赋值后的数值。为啥呢？因为我们仅对 变量 a 进行了 var 关键字变量声明，而 b 和 c 在未指定变量声明方式时，默认成为了全局变量，在根据赋值从右至左的顺序，c 和 b 相继赋值为 3。

好了，以上情况仅在非严格模式下出现，在严格模式下，因为 b 和 c 未指定变量声明关键字，会提示 `ReferenceError: b c is not defined`。

再来，那在非严格模式下，有没有啥潜在隐患呢？思考下面的代码块：

```javascript
var b = 1,
	c = 2;
(function () {
	var a,
		b,
		c = 3;
	a = b = c;
	console.log("a + b + c = " + (a + b + c));
})();
console.log("b + c = " + (b + c));
/*----------------------------------*/
var bb = 1,
	cc = 2;
(function () {
	var aa = (bb = cc = 3);
	console.log("aa + bb + cc = " + (aa + bb + cc));
})();
console.log("bb + cc = " + (bb + cc));
```

可以注意到，两者的区别就在立即执行函数内部 变量的声明和赋值方式，也因此最后输出的值产生了差异。在代码块 1 中，立即执行函数中，相继声明了 b , c 两个局部变量，之后的赋值也正是赋值给此两个局部变量；而代码块 2 中，则是对全局变量 bb ，cc 赋值，也就造成最终二者不同的输出值了。

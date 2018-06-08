---
title: JS-网传问题回顾
date: 2018-06-07 11:41:31
tags: [javascript]
---

# 问题
* 使用typeof bar ===“object”来确定bar是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？
* let a = b = c = 'value'; 方式声明赋值时，有没有潜在风险？



# 答案

## 使用  typeof  bar === "object" 来确定bar是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？

之所以使用 `typeof bar === object"` ，是因为大多数情况下我们想检测一个变量是否为一个对象。但是当待检测的变量为`null` ，或者赋值方式为以下几种方式时，如果没有深入了解数据类型的话，则会有挠头的姿态出现了：

```javascript
let a = new Date();
let b = new Number(110);
let c = new String('hello world');
let d = new Boolean(true);
let e = 'just string';

console.log(typeof null);
console.log(typeof a,a,a.toString());
console.log(typeof b,b,b.toString());
console.log(typeof c,c,c.toString());
console.log(typeof d,d,d.toString());
console.log(typeof e);
```

这里当值为 `null`，或者以 `new`关键字形式赋值后，输出的值均为 object。这里不深究数据类型，但如果我们想确切的检测一个变量的类型时，可以参考 jQuery的实现方式：

```javascript
/*此处为非jQuery，但参考jQuery而来的实现方式代码块*/
const class2type = {};
"Boolean Number String Function Array Date RegExp Object Error Symbol".split(" ").map(function(item){
    class2type["[object "+item+"]"] = item.toLowerCase();
});
function type( obj ) {
	if ( obj == null ) {
		return obj + "";
	}

	// Support: Android <=2.3 only (functionish RegExp)
	return typeof obj === "object" || typeof obj === "function" ?
		class2type[ toString.call( obj ) ] || "object" :
		typeof obj;
}

let a = new Date();
let b = new Number(110);
let c = new String('hello world');
let d = new Boolean(true);
let e = 'just string';

console.log(type(null));
console.log(type(a));
console.log(type(b));
console.log(type(c));
console.log(type(d));
console.log(type('just string'));
```

## let a = b = c = 'value'; 方式声明赋值时，有没有潜在风险？

```javascript
(function(){
  var a = b = c = 3;
})();
 
console.log("a defined? " + (typeof a !== 'undefined'),typeof a);
console.log("b defined? " + (typeof b !== 'undefined'),typeof b);
console.log("c defined? " + (typeof c !== 'undefined'),typeof c);
```

会打印出什么内容呢？想一想。

打印的结果是：a 为 undefined，b 和 c 为赋值后的数值。为啥呢？因为我们仅对 变量a 进行了 var 关键字变量声明，而 b 和 c 在未指定变量声明方式时，默认成为了全局变量，在根据赋值从右至左的顺序，c 和 b 相继赋值为 3。

好了，以上情况仅在非严格模式下出现，在严格模式下，因为 b 和 c 未指定变量声明关键字，会提示


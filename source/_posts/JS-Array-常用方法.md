---
title: JS-Array-常用方法
date: 2018-04-27 10:57:08
index_img: https://s3.ax1x.com/2021/03/08/6lY1dx.png
tags: [javascript]
---

## 静态方法

### Array.isArray(obj) ;

> 检测给定值是否为数组； 是则返回 true | 否则返回 false

```
console.log(Array.isArray([]));/* true */
console.log(Array.isArray(new Array(1,2)));/* true */
console.log(Array.isArray({}));/* false */
```

### 对操作数组本身进行修改的方法

- pop _删除并返回数组的最后一个元素_
- push _向末尾添加一个或多个元素，并返回新的长度_
- shift _删除并返回数据的第一个元素_
- splice _删除元素，并向数组添加元素_
- unshift _向开头添加一个或多个元素，并返回新的长度_
- reverse _颠倒数组中元素的顺序_
- sort _排序_
- fill _用指定值来填充数组_

### 对操作数组本身 无影响

- concat _连接两个或多个数组，并返回结果_
- join _将数组的所有元素放入一个字符串中，元素按指定的分隔符进行连接_
- slice _从已有的数组返回选定的元素_
- map _返回调用处理方法后的数组值_
- forEach _遍历数组所有值，并将值逐一传给回调函数_
- filter _返回一个新数组，新数组中为符合条件的所有值_
- find _返回符合条件的第一个值_
- findIndex _返回符合条件第一个值得下标索引_
- indexOf _返回指定值在数组中首次出现的位置_
- includes _数组是否包含指定值_
- every _遍历数组，检测是否所有值都符合给定的函数判断；全部符合返回 true_
- some _遍历数组，检测是否有符合给定函数的判断；有一个符合则返回 true_

## 对数组本身进行修改的方法

### pop

> 删除数组中的最后一个元素，并返回删除的元素

```
let arr = [1,2,'a',{b:2}];
console.log(arr);
/* [ 1, 2, 'a', { b: 2 } ] */

console.log(arr.pop());
/* { b: 2 } */

console.log(arr);
/* [ 1, 2, 'a' ] */

```

### push

> 向数组末尾添加一个或多个元素，并返回新的长度

```
let arr = [1,2];
let arrb = ['a','b'];
console.log(arr);
/* [ 1, 2 ] */

console.log(arr.push(...arrb));
/* length:4 */

console.log(arr);
/* [ 1, 2, 'a', 'b' ] */

arrb[0]='c';
console.log(arr);
/* [ 1, 2, 'a', 'b' ] */

console.log(arrb);
/* [ 'c', 'b' ] */
```

### shift

> 删除数组中的第一个元素，并返回删除的元素

```
let arr = [{a:1},2,3];
console.log(arr.shift());
/* {a:1} */

console.log(arr);
/* [2,3] */
```

### unshift

> 向数组的头部增加一个或多个元素，并返回数组新的长度

```
let arr = [{a:1},2,3];
let arrb = ['c',{name:'dan'}];
console.log(arr.unshift(...arrb));
/* length:5 */

console.log(arr);
/* [ 'c', { name: 'dan' }, { a: 1 }, 2, 3 ] */
```

### splice

> 向数组指定位置删除指定个数元素 或 添加元素，并返回删除元素的数组

- index : 操作的起始位置
- howmany : 删除的个数 0:不删除 | 不传:删除至数组末尾
- newItem,\*,newItems : 新增的元素

```
let arr = [1,2,3,4,5];
/*从数组第二位新增两个元素,注:纯新增必须设置第二个参数为 0*/
arr.splice(2,0,{a:1},234);
console.log(arr);
/* [ 1, 2, { a: 1 }, 234, 3, 4, 5 ] */
```

### reverse

> 颠倒数组的前后顺序，并返回颠倒排序后的数组

```
let arr = [1,2,3,4,5];
console.log(arr);
/* [ 1, 2, 3, 4, 5 ] */

arr.reverse();
console.log(arr);
/* [ 5, 4, 3, 2, 1 ] */
```

### sort

> 对数组进行排序 | 无参时 按字符编码顺序升序排序 | 有参：如下

- before 前一个元素
- next 后一个元素
  > 升序条件如下
- 如果 before 小于 next，在排序后的数组中 before 应该出现在 next 之前，则返回一个小于 0 的值
- 如果 before 等于 next，返回 0
- 如果 before 大于 next，在排序后的数组中 before 应该出现在 next 之后，则返回一个大于 0 的值
  > 降序条件如下
- 如果 before 小于 next，在排序后的数组中 before 应该出现在 next 之后，则返回一个大于 0 的值
- 如果 before 等于 next，返回 0
- 如果 before 大于 next，在排序后的数组中 before 应该出现在 next 之前，则返回一个小于 0 的值

```
let arr = [{name:'a',age:23},{name:'g',age:32},{name:'d',age:2},{name:'z',age:99},{name:'j',age:13},{name:'e',age:78},{name:'p',age:34},{name:'e',age:33}];
function sortBy(attr,ascORdesc = 'asc'){
	let ascORdescFlag = (ascORdesc== 'asc'?1:-1);
	return function sort(before,next){
		before = before[attr];
		next = next[attr];
		if(before<next){
			return -1*ascORdescFlag;
		}
		if(before>next){
			return 1*ascORdescFlag;
		}
		return 0;
	};
}
arr.sort(sortBy('age',22));
console.log(arr);
/*
[ { name: 'z', age: 99 },
 { name: 'e', age: 78 },
 { name: 'p', age: 34 },
 { name: 'e', age: 33 },
 { name: 'g', age: 32 },
 { name: 'a', age: 23 },
 { name: 'j', age: 13 },
 { name: 'd', age: 2 } ]
*/

arr.sort(sortBy('name'));
console.log(arr);
/*
[ { name: 'a', age: 23 },
 { name: 'd', age: 2 },
 { name: 'e', age: 78 },
 { name: 'e', age: 33 },
 { name: 'g', age: 32 },
 { name: 'j', age: 13 },
 { name: 'p', age: 34 },
 { name: 'z', age: 99 } ]
*/
```

### fill

> 将指定的值 替换到 数组中的指定位置

- value: 必填|填充的值
- start: 可选|填充的起始位置
- end: 可选|填充的结束位置

```
let arr = ['d','b','c',1,3];
arr.fill('hello',3,5);
console.log(arr);
/* [ 'd', 'b', 'c', 'hello', 'hello' ] */

arr.fill('world',2);
console.log(arr);
/* [ 'd', 'b', 'world', 'world', 'world' ] */

arr.fill('hello world');
console.log(arr);
/*
[ 'hello world',
 'hello world',
 'hello world',
 'hello world',
 'hello world' ]
*/
```

## 对数组本身无影响的方法

### concat

> 连接两个或多个数组

```
let arr = [1,{age:28},3],arrb=['a','b'],arrc = [];
arrc = arrc.concat(arr,arrb);
console.log(arrc);
/* [ 1, { age: 28 }, 3, 'a', 'b' ] */

arr[0] = 'hello world';
arr[1].age = 18;
console.log(arrc);
/*
注意: 原数组中，引用类型的值修改会造成返回的新数组值修改 [引用的为同一地址]
[ 1, { age: 18 }, 3, 'a', 'b' ]
*/

```

### join

> 将数组中得所有元素连接成字符串

- separator 连接各元素的分隔符；若不指定，默认为逗号连接

```
let arr = [-1,'a',['b','c',['d','e']],'123a'];
console.log(arr.join());
/* -1,a,b,c,d,e,123a */

console.log(arr.join(''));
/* -1ab,c,d,e123a */

/*打平嵌套数组
* 注：处理后，如元素组中的元素 为String类型的数字，处理后为Number类型
*/
function unwind(array){
	return arr.join(',').split(',').map((item)=>{return Number(item)?Number(item):item});
}
console.log(unwind(arr));
/* [ -1, 'a', 'b', 'c', 'd', 'e', 123 ] */
```

### slice

> 返回数组中指定的元素

- start | [起始下标] -1 为数组最后的元素
- end | [结束下标]

```
let arr = [1,'b',{c:'hello'},'d'];
let arrb = arr.slice(-1),arrc = arr.slice();
console.log(arrb);
/* [ 'd' ] */

console.log(arrc);
/* [ 1, 'b', { c: 'hello' }, 'd' ] */
```

### map

> 返回一个新数组,新元素为 原元素调用函数处理后的值
>
> - currentValue： 当前值[原数组] | 必须
> - index： 当前值的下标索引 | 可选
> - arr： 原数组对象 | 可选

```
let arr = [1,3,'4','b',{c:'hello'}];
let arrb = arr.map(function(item){
	return item*2;
});
console.log(arr);
/* [ 1, 3, '4', 'b', { c: 'hello' } ] */

console.log(arrb);
/* [ 2, 6, 8, NaN, NaN ] */
```

### forEach

> 遍历数组的每个元素，并将元素传递给回调函数
>
> - currentValue： 当前值[原数组] | 必须
> - index： 当前值的下标索引 | 可选
> - arr： 原数组对象 | 可选

```
/*
*数组去重
*此方法仅可用于基础类型的值 去重，引用类型无法去除
*/
function uniq(array){
	let returnArr = [],tmpMap = {};
	array.forEach((item)=>{
		if(!tmpMap[item]){
			returnArr.push(item);
			tmpMap[item] = 1;
		}
	});
	return returnArr;
}
let testArr = [1,1,2,2,2,3,3,3,3,'a','a','b','c',{d:'123'},{d:'456'}];
console.log(uniq(testArr));
/* [ 1, 2, 3, 'a', 'b', 'c', { d: '123' } ] */
```

### filter

> 返回一个新数组，新数组中的元素为符合判断条件的元素
>
> - currentValue： 当前值[原数组] | 必须
> - index： 当前值的下标索引 | 可选
> - arr： 原数组对象 | 可选

```
/* 数组去重
*此方法仅可用于基础类型的值 去重，引用类型无法去除
*/
let testArr = [1,1,2,2,2,3,3,3,3,'a','a','b','c'];
let reArr = testArr.filter((item,i,arr)=>{
	return arr.indexOf(item)===i;
});
console.log(reArr);
/* [ 1, 2, 3, 'a', 'b', 'c' ] */
```

### find

> 返回符合条件的 第一个值
>
> - currentValue： 当前值[原数组] | 必须
> - index： 当前值的下标索引 | 可选
> - arr： 原数组对象 | 可选

```
let testArr = [{age:11},{age:22},{age:33}];
let reArr = testArr.find((item,i,arr)=>{
	return item.age>15;
});
console.log(reArr);
/* { age: 22 } */
```

### findIndex

> 返回符合条件的 第一个值的下标索引
>
> - currentValue： 当前值[原数组] | 必须
> - index： 当前值的下标索引 | 可选
> - arr： 原数组对象 | 可选

```
let testArr = [{age:11},{age:22},{age:33}];
let reArr = testArr.findIndex((item,i,arr)=>{
	return item.age>15;
});
console.log(reArr);
/* 1 */
```

### indexOf

> 返回指定元素值的第一个下标索引

- item | 待检索的值
- start | 检索的起始位置

```
/* 数组去重
*此方法仅可用于基础类型的值 去重，引用类型无法去除
*/
let testArr = [1,1,2,2,2,3,3,3,3,'a','a','b','c'];
let reArr = testArr.filter((item,i,arr)=>{
	return arr.indexOf(item)===i;
});
console.log(reArr);
/* [ 1, 2, 3, 'a', 'b', 'c' ] */
```

### lastIndexOf

> 返回指定元素值的第一个下标索引

- item | 待检索的值

```
let testArr = [1,1,2,2,2,3,3,3,3,'a','a','b','c'];
console.log(testArr.lastIndexOf('a'));
/* 10 */
```

### includes

> 检测数组中是否包含指定元素，有则 true | 否则 false

- item | 待检测的值
- start | 检索的起始位置

```
let testArr = [11,22,33];
console.log(testArr.includes(22));
/* true */
```

### every

> 检测是否数组中的元素都符合指定的条件，都符合则返回 true | 一旦有一个不符合返回 false
>
> - currentValue： 当前值[原数组] | 必须
> - index： 当前值的下标索引 | 可选
> - arr： 原数组对象 | 可选

```
let testArr = [{age:11},{age:22},{age:33}];
let reArr = testArr.every((item,i,arr)=>{
	return item.age>15;
});
console.log(reArr);
/* false */
```

### some

> 检测数组中是否有一个满足条件的元素，只要有一个则返回 true | 一个都没有则返回 false
>
> - currentValue： 当前值[原数组] | 必须
> - index： 当前值的下标索引 | 可选
> - arr： 原数组对象 | 可选

```
let testArr = [{age:11},{age:22},{age:33}];
let reArr = testArr.some((item,i,arr)=>{
	return item.age>15;
});
console.log(reArr);
/* true */
```

### reduce

> 将数组中的每个值从左到右开始缩减，经函数处理后，最终返回一个值

- function
  - total 初始值, 或者计算结束后的返回值 | 必须
  - currentValue： 当前值[原数组] | 必须
  - index： 当前值的下标索引 | 可选
  - arr： 原数组对象 | 可选
- 可选。传递给函数的初始值

```
/*打平嵌套数组
* 使用条件仅为 待打平的数组为 二维数组
*/
let testArr = [1,['a','c'],3];
console.log(testArr.reduce((r,item)=>r.concat(item),[]));
/* [ 1, 'a', 'c', 3 ] */
/* 注：不提供默认值时：初始的默认值为 首位元素的值*/
```

### reduceRight

> 将数组中的每个值从右到左开始缩减，经函数处理后，最终返回一个值

- function
  - total 初始值, 或者计算结束后的返回值 | 必须
  - currentValue： 当前值[原数组] | 必须
  - index： 当前值的下标索引 | 可选
  - arr： 原数组对象 | 可选
- 可选。传递给函数的初始值

```
let testArr = [1,2,3,4,5,6];
console.log(testArr.reduce((total,item)=>{
	return total-item;
}));
/* 注：不提供默认值时：初始的默认值为 末位元素的值*/
/* -19 */
```

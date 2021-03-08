---
title: JS-Date-总结
date: 2018-08-01 10:32:08
index_img: https://s3.ax1x.com/2021/03/08/6lJ4PO.png
tags: [javascript]
---

> 在 javascript 中对日期的操作还是很多的，比如给日期控件设定个默认值，在 vue 中根据 Date 值返回对应毫秒数，使用场景还是很多的，那么个人常用的场景有如下：

- 给日期控件设定默认值时，需要指定默认的日期时间，例如设定默认日期时间为 `2008-10-08 12:12:12`

```javascript
let defaultDate = new Date("2008-10-08 12:12:12");
let defaultDateMS = Date.parse(defaultDate);
console.log(defaultDate); //Wed Oct 08 2008 12:12:12 GMT+0800
console.log(defaultDateMS); //1223439132000
```

- 默认值为当前的前一天的早上六点

```javascript
let defaultDate = new Date(new Date().setHours(6, 0, 0));
defaultDate.setDate(defaultDate.getDate() - 1);
console.log(defaultDate); //Tue Jul 31 2018 06:00:00 GMT+0800
console.log(Date.parse(defaultDate)); //1532988000000
```

- 默认值为上月的第一天或最后一天

```javascript
let lastMonthFirstDay = new Date(new Date().setHours(0, 0, 0));
lastMonthFirstDay.setMonth(lastMonthFirstDay.getMonth() - 1);
lastMonthFirstDay.setDate(1);
console.log(lastMonthFirstDay); //Sun Jul 01 2018 00:00:00 GMT+0800
console.log(Date.parse(lastMonthFirstDay)); //1530374400000

let lastMonthLastDay = new Date(new Date().setHours(0, 0, 0));
lastMonthLastDay.setDate(1);
lastMonthLastDay.setDate(lastMonthLastDay.getDate() - 1);
console.log(lastMonthLastDay); //Tue Jul 31 2018 00:00:00 GMT+0800
console.log(Date.parse(lastMonthLastDay)); //1532966400000
```

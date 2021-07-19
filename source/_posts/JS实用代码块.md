---
title: JS实用代码块
date: 2021-07-14 15:50:20
tags: [javascript]
---

# 处理从 Excel 中复制的数据

[参考链接](https://developer.mozilla.org/en-US/docs/Web/API/ClipboardEvent)

```javascript
document.addEventListener("paste", function (event) {
	var html = event.clipboardData.getData("text/html"); // 获取 html格式内容
	var $doc = new DOMParser().parseFromString(html, "text/html"); //转为 html格式
	var $trs = Array.from($doc.querySelectorAll("table tr td")); // 筛选单元格
	var arr = [];
	$trs.forEach(function (item) {
		// 取单元格文本值
		var val = item.textContent.replace("\n", "");
		val = isNaN(val) ? "" : val;
		arr.push(val);
	});
	console.log(arr);
});
```

# hexo 支持 PWA

[参考链接](https://www.imgyh.com/posts/74ba30cc/)
[How to Scope Your Free Range PWA Service Workers](https://dev.to/njromano/how-to-scope-your-pwa-service-workers-1n6m)
[PWA](https://blog.ihoey.com/posts/javascript/PWA/2019-02-18-progressive-web-apps-blog.html)

- 找个支持 PWA 的主题，例如 [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly)
- [制作各平台的图标](https://realfavicongenerator.net/)
- 添加 manifest.json
- 注入 service work

# drop 生效条件

页面很多的地方是无效触发 drop 事件的，因此想要触发，在 `dragenter` `dragover` 时需要禁用默认行为

```
event.preventDefault();

function doDragOver(event) {
  const isLink = event.dataTransfer.types.includes("text/uri-list");
  if (isLink) {
    event.preventDefault();
  }
}
```

[参考链接](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API/Drag_operations#droptargets)

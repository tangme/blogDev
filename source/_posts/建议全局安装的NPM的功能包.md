---
title: 建议全局安装的NPM的功能包
date: 2019-11-25 17:00:22
tags:
---

# live-server
> 将当前目录，设置为静态服务；之后可快速的通过本机ip或localhost的http形式，访问目录

```
/*
 * 默认情况下，post请求时，是不会有返回；
 * 如须支持，请在安装 live-server的node_modules目录下
 * 找到 live-server 目录并进入，打开 index.js
 * 修改第 42 行，如下：
 */

if (req.method !== "GET" && req.method !== "POST" && req.method !== "HEAD") return next();
```

# rimraf
> 删除指定文件或目录；相比系统自带的delete，在删除目录层级很深的情形下，rimraf速度提升不少
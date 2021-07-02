---
title: nodejs 相关
date: 2019-12-09 08:57:48
index_img: https://s3.ax1x.com/2021/02/22/yHe27R.png
cover: https://s3.ax1x.com/2021/02/22/yHe27R.png
tags:
---

# 科普扫盲

## npx

> npx 会首先在当前目录下查找模块，如果有则执行 如果当前目录下没有指定模块，则将模块下载到临时文件，然后执行模块，执行后删除

- [轻松地运行本地命令](http://nodejs.cn/learn/the-npx-nodejs-package-runner#%E8%BD%BB%E6%9D%BE%E5%9C%B0%E8%BF%90%E8%A1%8C%E6%9C%AC%E5%9C%B0%E5%91%BD%E4%BB%A4)
- [无需安装的命令执行](http://nodejs.cn/learn/the-npx-nodejs-package-runner#%E6%97%A0%E9%9C%80%E5%AE%89%E8%A3%85%E7%9A%84%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C)
- [使用不同的 Node.js 版本运行代码](http://nodejs.cn/learn/the-npx-nodejs-package-runner#%E4%BD%BF%E7%94%A8%E4%B8%8D%E5%90%8C%E7%9A%84-nodejs-%E7%89%88%E6%9C%AC%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81)
- [直接从 URL 运行任意代码片段](http://nodejs.cn/learn/the-npx-nodejs-package-runner#%E7%9B%B4%E6%8E%A5%E4%BB%8E-url-%E8%BF%90%E8%A1%8C%E4%BB%BB%E6%84%8F%E4%BB%A3%E7%A0%81%E7%89%87%E6%AE%B5)

## nvm

> Node Version Manager
> Node 版本管理器，在开发环境下，方便让一台电脑安装不同版本的 node

## 调试

> node --inspect-brk

立即进入 debugger 模式，chrome 地址栏 chrome://inspect 进入 debugger 页面

# 建议全局安装的 NPM 的功能包

## nrm

> NPM registry manager
> NPM 包源管理器，默认的`https://registry.npmjs.org/`包源地址，国内常常响应缓慢，可以通过`nrm` 切换至 `taobao`源，快速方便

## live-server

> 将当前目录，设置为静态服务；之后可快速的通过本机 ip 或 localhost 的 http 形式，访问目录

```
/*
 * 默认情况下，post请求时，是不会有返回；
 * 如须支持，请在安装 live-server的node_modules目录下
 * 找到 live-server 目录并进入，打开 index.js
 * 修改第 42 行，如下：
 */

if (req.method !== "GET" && req.method !== "POST" && req.method !== "HEAD") return next();
```

## rimraf

> 删除指定文件或目录；相比系统自带的 delete，在删除目录层级很深的情形下，rimraf 速度提升不少

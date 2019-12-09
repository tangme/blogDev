---
title: nodejs 相关
date: 2019-12-09 08:57:48
tags:
---

# nvm
> Node Version Manager
> Node 版本管理器，在开发环境下，方便让一台电脑安装不同版本的node


# 建议全局安装的NPM的功能包

## nrm
> NPM registry manager
> NPM 包源管理器，默认的`https://registry.npmjs.org/`包源地址，国内常常响应缓慢，可以通过`nrm` 切换至 `taobao`源，快速方便

## live-server
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

## rimraf
> 删除指定文件或目录；相比系统自带的delete，在删除目录层级很深的情形下，rimraf速度提升不少

---
title: Vue与Electron成长记
date: 2019-11-28 16:33:10
index_img: https://s3.ax1x.com/2021/03/10/6J0ayq.jpg
tags:
---

> vue 项目完成后，如果想打包为应用程序，可以使用 Electron，从零开始，上手步骤如下：

- [安装 Vue CLI](https://cli.vuejs.org/zh/guide/installation.html)
- 通过 `vue ui` 新建项目
- 项目安装 `vue-cli-plugin-electron-builder` 插件
- 进入 `vue ui` 仪表板
  - 进入 `任务` 菜单
    - 选择 `electron:build`
      - 设置 `变量` 后，点击 `运行` 即可尝鲜打包后的桌面应用

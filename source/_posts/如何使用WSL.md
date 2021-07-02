---
title: 如何使用WSL
date: 2021-04-20 15:46:45
index_img: https://z3.ax1x.com/2021/05/07/g1aOHS.jpg
cover: https://z3.ax1x.com/2021/05/07/g1aOHS.jpg
tags:
---

WSL(Windows Subsystem for Linux)，在 Windows 下提供 Linux 运行环境，不会带来传统虚拟机与双系统的累赘。

正常情况下想在 Windows 下使用 Linux，得安装虚拟机，然后在虚拟机中运行 Linux，虚拟机本身带来了开销。或者制作双系统，但又带来了 Windows 与 Linux 两系统间切换的麻烦。所以 WSL 解决了以上两个痛点。

[安装教程](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)直接参考官网的即可，非常清晰。

两篇参考文章，还没用到，留作备份

[WSL2 来了！但是能正常使用并不简单](https://zhuanlan.zhihu.com/p/144583887)

[关于使用 WSL2 出现“参考的对象类型不支持尝试的操作”的解决方法](https://zhuanlan.zhihu.com/p/151392411)

使用**VS Code**编辑现有的项目，有下面几种方式
1、进入*WLS*下的项目目录，运行 `code .` 即可。

默认情况下**WSL**没有设置**root**密码，可如下来设置

> $ sudo passwd root

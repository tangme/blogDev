---
title: Linux常识扫盲
date: 2021-05-08 10:42:44
tags:
---

普通用户理论上很少接触 **Linux** 系统，毕竟 **Windows** 的图形化操作方便直接，普罗大众；但作为开发人员，不论你是前端还是后端，**Linux** 是一个绕不开的话题，最终都会须要学习它。

本文不谈 **Linux** 任何原理，因为一上来就跟你掰扯这些，是感受不到实际操作快乐的。

现在假定你还是 **Windows** 为生产力的输出，想体验 **Linux**,那么可以使用 **WSL** 来初步上手。

我们假定的场景是：

1.  有了 Linux 环境
2.  进入目录，创建新的项目目录
3.  创建新的文件
4.  对文件进行编辑
5.  发布项目
6.  访问项目

好了，开始吧~

## 开始练手

### 新建项目目录

假定我们想在 `/home/`目录下新建 `helloworld`目录，命令如下：

> $ cd /home/
> $ mkdir helloworld

### 新建项目文件

在 `helloworld` 目录下，新建 `index.html` 文件

> $ cd helloworld
> $ :>index.html

### 修改文件内容

> $ vim index.html

```
//在编辑器中输入
this is index.html file.

//按 esc 后
//输入  :wq
```

## Linux 常用命令解释与全称

### su

- swith user
- 切换用户

切换至 root (超级管理)用户

> $ su

切换至 xiaoming (普通)用户

> $ su xiaoming

### pwd

- print working directory
- 显示当前的(工作)目录

### cd

- change directory
- 切换(改变) 目录

回到上级目录

> $ cd ..

进入当前目录下的 home 文件夹

> $ cd home/

### mkdir

- make directory
- 新建目录

### rmdir

- remove directory
- 删除目录

### ls

- list files
- 列出当前目录下的文件

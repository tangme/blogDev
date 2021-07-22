---
title: RN-相关
date: 2021-03-11 09:27:04
index_img: https://s3.ax1x.com/2021/03/11/6Y6HRU.png
cover: https://s3.ax1x.com/2021/03/11/6Y6HRU.png
tags: [RN, react native]
---

# 安卓(Android)

## gradle

前端思维理解就是 npm + gulp/grunt
gradle 可以用来安装第三方包，并且将项目打包

## adb (Android Debug Bridge) 相关命令

### adb devices

![查看已连接的设备](https://s3.ax1x.com/2021/03/11/6Ygh40.jpg)

### adb reverse

将电脑端口映射到移动设备

> adb reverse tcp:8081 tcp:8081

### 连接了多个设备，对指定设备进行操作

对指定设备安装应用

> adb -s [devicecode] install [package.app]
> adb -s RCJ6R20921001617 install com.notive

对指定设备映射端口

> adb -s [devicecode] reverse tcp:[port] tcp:[port]
> adb -s RCJ6R20921001617 reverse tcp:8081 tcp:8081

### 查看手机当前启动 app 的应用名和包名

> adb shell dumpsys window w |findstr \/ |findstr name=

![查看当前打开应用的包名](https://s3.ax1x.com/2021/03/11/6YR0l8.jpg)

### 设备不能进入调试模式

1. 撤销 usb 调试授权
2. 重新开启 usb 调试
3. 重新连接充电线，选择读取文件模式(非必要)

### wifi (无数据线连接)

1. 摇晃安卓设备
2. 点击**change bundle**，并输入开发本机 ip 地址与端口号

### react-devtools

> adb shell input keyevent 82

点击 **Toggle Inspector ** 唤起 _react-devtools_ 应用

# 项目配置

## [设置路径别称 (alias)](https://mtateam.medium.com/how-to-use-import-aliases-with-react-native-and-vs-code-dadb246674c7)

> npm i -D metro-react-native-babel-preset

> npm i -D babel-plugin-module-resolver

```
//在babel.config.js中设置

module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    [
      'module-resolver',
      {
        root: ['./src'],
        extensions: [
          '.ios.ts',
          '.android.ts',
          '.ts',
          '.ios.tsx',
          '.android.tsx',
          '.tsx',
          '.jsx',
          '.js',
          '.json',
        ],
        alias: {
          //'@navigation': './src/navigation',
          //'@components': './src/components',
          //'@assets': './assets',
          '@': './src/',
        },
      },
    ],
  ],
};
```

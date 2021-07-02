---
title: Flex-布局
date: 2021-03-25 15:29:13
cover: https://z3.ax1x.com/2021/07/02/R6iegg.jpg
tags:
---

三列布局

<section style="display:flex;">
  <div style="width:50px;background-color:aliceblue;">左边栏</div>
  <div style="flex:1;background-color:chocolate;">
    <ul>
      <li>左右两侧边栏高度，跟随内容高度</li>
      <li>内容区域为不固定高度</li>
      <li>我是来增加高度的</li>
    </ul>
  </div>
  <div style="width:50px;background-color:antiquewhite;">右边栏</div>
</section>

</br>

<details>
<summary>点击显示源代码</summary>

```
<section style="display:flex;">
  <div style="width:50px;background-color:aliceblue;">左边栏</div>
  <div style="flex:1;background-color:chocolate;">
    <ul>
      <li>左右两侧边栏高度，跟随内容高度</li>
      <li>内容区域为不固定高度</li>
      <li>我是来增加高度的</li>
    </ul>
  </div>
  <div style="width:50px;background-color:antiquewhite;">右边栏</div>
</section>

```

</details>

## 子项

### flex-grow

当父元素有剩余空间时，分配至子项的比例

#### 子项 flex-grow 和值大于等于 1 的情景

增加的宽度(大小)为：父元素剩余空间 `*` 子项自身 grow 值 `/` 所有子项 grow 合计值

#### 子项 flex-grow 和值小于 1 的情景

增加的宽度(大小)为：父元素剩余空间 `*` 子项自身 grow 值

### flex-shrink

父元素空间不足时(即：一根轴上不能容纳所有的子项)，缩小子项的比例

#### 子项 flex-shrink 和值大于 1 的情景

减小的宽度：溢出空间大小 `*` 子项自身 shrink 值 `*` 子项自身宽度 `/` 总权重值（各子项宽度\*各子项 shrink 值之和）

#### 子项 flex-shrink 和值小于 1 的情景

减小的宽度：`(` 溢出空间大小 `*` 各子项 shrink 和值 `)` `*` 子项自身 shrink `*` 子项自身宽度 `/` 总权重值（各子项宽度\*各子项 shrink 值之和）

### flex

**flex** 是 _flex-grow_ _flex-shrink_ _flex-basis_ 的简写

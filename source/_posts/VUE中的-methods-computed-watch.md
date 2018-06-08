---
title: VUE中的-methods-computed-watch
date: 2018-06-04 09:53:10
tags:
 - vue
---

[原文地址](https://css-tricks.com/methods-computed-and-watchers-in-vue-js/#article-header-id-2)

`methods`,`computed`和`watch`相互之间区别的易辨认性及三者的易用性是我喜欢使用Vue的原因之一。如不了解以上三者，那么很难发挥Vue的所有潜在功能。在我看来，大多数对此(Vue)框架有困惑的人，同时对以上三者的区别也有着疑惑，那么现在让我们来探究下。

如果你只须一个结论，或者没有时间通读全文，以下则是总结：
* **methods**: 如同词语自身描述的一样。它们是处理对象的方法，通常来说是Vue实例本身，或者是Vue组件。
* **computed**: 这些属性第一眼看起来，像是被当作方法使用，但实则不然。在Vue中，我们使用 `data` 来跟踪特定属性的变化。computed 属性允许我们定义一个属性以 `data` 同样的方式来使用，但不同是，拥有一套自定的逻辑基于已有的缓存依赖项上。你可以把计算属性认为是 `data的一种逻辑处理后的方式`。
* **watch**: 其能允许你一览反应系统。我们提供了些钩子来观察存储在 Vue 中的任何属性。如果我们想在每时每刻一有变化时就增加一些功能，或者相应特定的变化，我们可以监听一个属性，然后，赋予些逻辑。这就是说，监听器`必须匹配`我们所观察的属性。

如果以上的措辞使你困惑，别着急。接下来我们将深入讲解，以希望能解决你的所有疑惑。如果你已对 JavaScript 很熟悉，methods对你应毫无压力，(当然除了一两个值得留心的小点)。那么可以直接游览 computed 和 watch 章节。

# Methods
Methods 应该是我们在Vue中使用的最多了东西了。They’re aptly named as, in essence, we’re hanging a function off of an object。在给事件响应指令，亦或重构一个函数进行复用的情形下，方法都尤为实用。例如，你能在一个方法中调用另一个方法。也能在生命周期钩子事件中调用方法。使用很是灵巧。
以下为一个示例演示：
```html
<!-- html code -->

<code class="language-css"><div id="app">
  <button @click="tryme">Try Me</button>
  <p>{{ message }}</p>
</div>
```

```javascript
/* javascript code*/
new Vue({
  el: '#app',
  data() {
    return {
      message: null
    }
  },
  methods: {
    tryme() {
      this.message = Date()
    }
  }
})
```
我们也能直接在事件中中执行逻辑指令，如 `<button @click="message = Date()">Try Me</button>`，在这个小例子中也能顺利执行。但是呢，随着我们开发应用的复杂度增长，更常见的作法是如我们上面例子所展示的，把业务代码抽取出，以获得更好的代码可读性与可维护性。在Vue中使用指令时，也有一些限制,例如，表达式是允许的，但是声明则不行。

你可能注意到了，我们在Vue实例或组件中调用此方法，并且在此方法中可以访问所有的data数据，此例中为,`this.message`。在指令中不必非得像调用函数那样调用方法。例如，`@click=”methodName()”` 可以引用为`@click=”methodName”`，当然如需传递参数时，则是`@click=”methodName(param)”`。

使用指令调用方法很赞的另一个原因是，我们能够使用一些修饰符。下例中一个很有用的修饰符为`.prevent`，此修饰符将阻止默认提交事件后刷新页面的情形，例子如下：
```html
<form v-on:submit.prevent="onSubmit"></form>
```
更多信息，请[移步到这](https://vuejs.org/v2/api/?#v-on)

# Computed

计算属性在控制已有的数据上是非常有用的。当你相对一个大量数据进行排序且不想每次获取计算后的返回值时，可以了解下 计算属性。

以下为一些适当使用计算属性的条件，但不局限与此：

* 在用户输入信息后，需要对已有的大量数据更新，如过滤显示符合输入的信息内容
* 从 Vuex 状态管理器中采集信息。
* 表单验证
* 根据用户所想看的可视化数据信息展示

对于理解Vue，计算属性是很重要的一部分。计算属性的计算值会根据它们所依赖的数据进行缓存，并只有当符合特定条件时更新。当合理使用计算属性时，其是非常高效和有用的。此外，已经有很多健壮的库和函数提供给我们来处理业务逻辑部分，以降低编程时的代码量。

计算属性并不像方法那样来的使用，尽管它两看起来很相似。计算属性是：你在一个函数中编写逻辑代码并返回符合逻辑的值，但是 此方法的函数名 将会变成一个属性，就像你在应用使用 `data`里的属性一样。

如果我们想在一个大量的英雄名称列表中，通过输入关键字来过滤内容，我们可以采取下面的方式，并且通过这个简单的例子让你对计算属性有个初步的概念。首先，我们的使用存储在 `data`  中的 `names` 属性，将列表内容输出在模板中：

```javascript
new Vue({
  el: '#app',
  data() {
    return {
      names: [
        'Evan You',
        'John Lindquist',
        'Jen Looper',
        'Miriam Suzanne',
        ...
      ]
    }
  }
})
```

```html
<div id="app">
  <h1>Heroes</h1>
  <ul>
    <li v-for="name in names">
      {{ name }}
    </li>
  </ul>
</div>
```

现在，给这些名称添加一些过滤代码。首先，给文本输入框绑定`v-mode`，且初始为空字符串值，当然最终我们会使用文本输入框中的值去匹配和过滤后我们的名称列表。给输入文本框绑定的属性值为`findName`，其与`data`中的值保持引用关联。

```html
<label for="filtername">Find your hero:</label>
<input v-model="findName" id="filtername" type="text" />
```

```javascript
data() {
  return {
    findName: '',
    names: [
      'Evan You',
      'John Lindquist',
      ...
    ]
  }
}
```

接下来，我们创建一个计算属性，其会根据用户在文本输入框中键入的内容，来过滤掉只符合`findName`属性值相关的名称内容。这里可以看到我使用了正则表达式来弱化了大小写的敏感度，因为作为一个用户，多数情况下是不会键入符合大小写规则的内容。

```javascript
computed: {
  filteredNames() {
    let filter = new RegExp(this.findName, 'i')
    return this.names.filter(el => el.match(filter))
  }
}
```

随后我们更新页面模板中的输出项，将：

```html
<ul>
  <li v-for="name in names">
    {{ name }}
  </li>
</ul>
```

调整为：

```html
<ul>
  <li v-for="name in filteredNames">
    {{ name }}
  </li>
</ul>
```

可以看到，我们每次键入任何的信息，都会展示过滤后的内容。可见我们只须键入几行代码，而不须引用其它的类库，就能顺利的实现功能需求。

我不会告诉你，这帮我省下了多少事件。如果你在使用Vue，而未[合理的使用计算属性](https://vuejs.org/v2/guide/computed.html#Computed-Properties) ，赶紧来试试，定让你开心的飞起来。
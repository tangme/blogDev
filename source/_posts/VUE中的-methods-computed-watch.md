---
title: VUE中的-methods-computed-watch
date: 2018-06-04 09:53:10
index_img: https://s3.ax1x.com/2021/02/23/ybdhY6.jpg
tags:
  - vue

categories: 译文
---

[原文地址](https://css-tricks.com/methods-computed-and-watchers-in-vue-js/#article-header-id-2)

`methods`,`computed`和`watch`的易用性与相互之间明确使用场景的定位，是我喜欢使用 Vue 的原因之一。如不了解以上三者，那么很难发挥 Vue 的所有潜在功能。在我看来，大多数对此(Vue)框架有困惑的人，同时对以上三者的区别也有着疑惑，那么现在让我们来探究下。

如果你只须一个结论，或者没有时间通读全文，以下则是总结：

- **methods**: 如同词语自身描述的一样。它们是处理对象的方法，通常来说是 Vue 实例本身，或者是 Vue 组件。
- **computed**: 这些属性第一眼看起来，像是被当作方法使用，但实则不然。在 Vue 中，我们使用 `data` 来跟踪特定属性的变化。computed 属性允许我们定义一个属性以 `data` 同样的方式来使用，但不同是，拥有一套自定的逻辑基于已有的缓存依赖项上。你可以把计算属性认为是 `data被逻辑处理后的形式`。
- **watch**: 其能允许你一览反应系统。我们提供了些钩子来观察存储在 Vue 中的任何属性。如果我们想在每时每刻一有变化时就增加一些功能，或者相应特定的变化，我们可以监听一个属性，然后，赋予些逻辑。这就是说，监听器`必须匹配`我们所观察的属性。

如果以上的措辞使你困惑，别着急。接下来我们将深入讲解，以希望能解决你的所有疑惑。如果你已对 JavaScript 很熟悉，methods 对你应毫无压力，(当然除了一两个值得留心的小点)。那么可以直接游览 computed 和 watch 章节。

# Methods

Methods 应该是我们在 Vue 中使用的最多了东西了。They’re aptly named as, in essence, we’re hanging a function off of an object。在给事件响应指令，亦或重构一个函数进行复用的情形下，方法都尤为实用。例如，你能在一个方法中调用另一个方法。也能在生命周期钩子事件中调用方法。使用很是灵巧。
以下为一个示例演示：

[点击查看在线 DEMO](//codepen.io/sdras/embed/caf96f7c14dc52323b97dd9845a0bf64?height=300&theme-id=1&slug-hash=caf96f7c14dc52323b97dd9845a0bf64&default-tab=result&user=sdras&embed-version=2&pen-title=Slim%20example%20of%20methods)

```html
<!-- html code -->

<code class="language-css"
	><div id="app">
		<button @click="tryme">Try Me</button>
		<p>{{ message }}</p>
	</div></code
>
```

```javascript
/* javascript code*/
new Vue({
	el: "#app",
	data() {
		return {
			message: null,
		};
	},
	methods: {
		tryme() {
			this.message = Date();
		},
	},
});
```

我们也能直接在事件中中执行逻辑指令，如 `<button @click="message = Date()">Try Me</button>`，在这个小例子中也能顺利执行。但是呢，随着我们开发应用复杂度的增长，更常见的作法是如我们上面例子所展示的，把业务代码抽取出，以获得更好的代码可读性与可维护性。在 Vue 中使用指令时，也有一些限制，例如：表达式是允许的，但是声明则不行。

你可能注意到了，我们在 Vue 实例或组件中调用此方法，并且在此方法中可以访问所有的 data 数据，此例中为,`this.message`。在指令中不必非得像调用函数那样调用方法。例如，`@click=”methodName()”` 可以引用为`@click=”methodName”`，当然如需传递参数时，则是`@click=”methodName(param)”`。

使用指令调用方法很赞的另一个原因是，我们能够使用一些修饰符。下例中一个很有用的修饰符为`.prevent`，此修饰符将阻止默认提交事件后刷新页面的情形，例子如下：

```html
<form v-on:submit.prevent="onSubmit"></form>
```

更多信息，请[移步到这](https://vuejs.org/v2/api/?#v-on)

# Computed

计算属性在控制处理已有数据上是非常有用的。当你需要对一个大量数据进行排序又不想每次获取计算后的返回值时，可以了解下计算属性。

以下为一些适当使用计算属性的条件，但不局限与此：

- 在用户输入信息后，需要对已有的大量数据更新，如过滤显示符合输入内容的信息
- 从 Vuex 状态管理器中采集信息。
- 表单验证
- 根据用户所想看的可视化数据信息展示

对于理解 Vue，计算属性是很重要的一部分。计算属性的计算值会根据它们所依赖的数据进行缓存，并只有当符合特定条件时更新。当合理使用计算属性时，其是非常高效和有用的。此外，已经有很多健壮的库和函数提供给我们来处理业务逻辑部分，以降低编程时的代码量。

计算属性并不像方法那样来的使用，尽管它两看起来很相似。计算属性是：你在一个函数中编写逻辑代码并返回符合逻辑的值，但是 此方法的函数名 将会变成一个属性，就像你在应用使用 `data`里的属性一样。

如果我们想在一个大量的英雄名称列表中，通过输入关键字来过滤内容，我们可以采取下面的方式，并且通过这个简单的例子让你对计算属性有个初步的概念。首先，我们的使用存储在 `data` 中的 `names` 属性，将列表内容输出在模板中：

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
		<li v-for="name in names">{{ name }}</li>
	</ul>
</div>
```

现在，给这些名称添加一些过滤代码。首先，给文本输入框绑定`v-mode`，且初始为空字符串值，当然最终我们会使用文本输入框中的值去匹配和过滤后我们的名称列表。给输入文本框绑定的属性值为`findName`，其与`data`中的值保持引用关联。

```html
<label for="filtername">Find your hero:</label> <input v-model="findName" id="filtername" type="text" />
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
	<li v-for="name in names">{{ name }}</li>
</ul>
```

调整为：

```html
<ul>
	<li v-for="name in filteredNames">{{ name }}</li>
</ul>
```

可以看到，我们每次键入任何的信息，都会展示过滤后的内容。可见我们只须键入几行代码，而不须引用其它的类库，就能顺利的实现功能需求。

我不会告诉你，这帮我省下了多少事件。如果你在使用 Vue，而未[合理的使用计算属性](https://vuejs.org/v2/guide/computed.html#Computed-Properties) ，赶紧来试试，定让你开心的飞起来。

# Watchers

Vue 有着很好的抽象体系设计，不过基本上每个编程人员在使用抽象类时，都会有遇到绕不过的坎而不爽。但也正式基于此痛点，Vue 提供给我们在响应体系中更深度的操作能力，以便我们通过设置钩子来观察任何数据的改变。讲真，这实在太有用了，因为作为一个应用的开发者，大多数时候我们是对数据的变化而响应相关操作的。

Watchers(侦听器) 允许我们编写更多声明式代码。以简化我们自己编写的代码量。Vue 已在底层实现了此功能，因此我们能在 `data`，`computed` 或 `props` 中跟踪任何数值的改变，来举个例。

Watchers(侦听器) 在监测属性值改变时，执行特定的业务逻辑代码非常好用(我第一次是从 [Chris Fritz](https://twitter.com/chrisvfritz) 听到这种操作方式的，但是他说他也是从别处体验到的 ☺️)。多数情况下，通过检测属性的改变来执行业务逻辑，这也正是 与 计算属性不同的地方。

现在来跑一个简单的例子，感受下 watch 的效果

```html
<div id="app">
    <input type="number" v-model.number="counter"></input>
</div>
```

```javascript
new Vue({
	el: "#app",
	data() {
		return {
			counter: 0,
		};
	},
	watch: {
		counter() {
			console.log("The counter has changed!");
		},
	},
});
```

如上面代码所示，我们在`data`中设置了`counter`，并将此属性名称作为方法名称，在`watch`中配置`counter`，以便我们能监测设置的`counter`属性值，最后我们可以看到，一旦`counter`数值发生改变，控制台都有输出。

# Transitioning State With Watchers

如果监测的状态标识符很简单，那么可以使用 watch(侦听器)来实现一个根据状态值改变的过度效果。以下是一个使用 Vue 来完成的柱状图表。随着数值的变化，watch(侦听器)将通过过度效果来更新图表。

SVG 如下面的例子一样很好使用，因为其以 数据 来构建。

[点击查看 DEMO](//codepen.io/sdras/embed/OWZRZL?height=578&theme-id=1&slug-hash=OWZRZL&default-tab=result&user=sdras&embed-version=2&pen-title=Chart%20made%20with%20Vue%2C%20Transitioning%20State)

```javascript
watch: {
  selected: function(newValue, oldValue) {

    var tweenedData = {}

    var update = function () {
      let obj = Object.values(tweenedData);
      obj.pop();
      this.targetVal = obj;
    }

    var tweenSourceData = { onUpdate: update, onUpdateScope: this }

    for (let i = 0; i < oldValue.length; i++) {
      let key = i.toString()
      tweenedData[key] = oldValue[i]
      tweenSourceData[key] = newValue[i]
    }

    TweenMax.to(tweenedData, 1, tweenSourceData)
  }
}
```

这里干了些啥呢？

- 首先我们创建了一个对象，其会通过动画库来更新。
- 然后这里可以看到一个`update`方法，
- 接下来创建一个对象来接收
- 接着创建一个 for 循环，将当前下编转换为字符串类型
- 但我们只对指定的键值执行此操作

我们也能在侦听器中使用动画来实现一个时差刻度盘。因为我时不时的会外出溜达，并且我的小伙伴也分散在不同的地方，所以需求之一就是能保证一个我们各自的当地时间都能在线，并且体现出是白天还是夜晚。

[点击查看 DEMO](//codepen.io/sdras/embed/RZGqxR?height=700&theme-id=1&slug-hash=RZGqxR&default-tab=result&user=sdras&embed-version=2&pen-title=Vue%20Time%20Comparison)

这里我们监听 选中的属性值，根据当前时间去触发不同的方法来改变 各个区域时间，其会通过色调，饱和度，和其它过度效果来展现。在之前的实现方式中，我们是通过下拉事件，而现在是在侦听器的方法中了。

```javascript
watch: {
  checked() {
    let period = this.timeVal.slice(-2),
      hr = this.timeVal.slice(0, this.timeVal.indexOf(':'));

    const dayhr = 12,
      rpos = 115,
      rneg = -118;

    if ((period === 'AM' && hr != 12) || (period === 'PM' && hr == 12)) {
      this.spin(`${rneg - (rneg / dayhr) * hr}`)
      this.animTime(1 - hr / dayhr, period)
    } else {
      this.spin(`${(rpos / dayhr) * hr}`)
      this.animTime(hr / dayhr, period)
    }

  }
},
```

关于 watchers(侦听器)还有很多其它有趣的使用方式，比如：

从输入，到异步更新，再到动画，watchers(侦听器)在更新方面能做的事实在是太多了。如果你对 Vue 是如何处理响应工作感到好奇， [这部分指南](https://vuejs.org/v2/guide/reactivity.html)会十分有帮助。如果你想更加全面的了解 响应，我十分推荐  [Andre Staltz' post](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754) 和 Mike Bostock’s [A Better Way to Code](https://medium.com/@mbostock/a-better-way-to-code-2b1d2876a3a0)的响应章节部分。

# 总结

希望通过以上各部分的讲解，有助于我们正确的使用三者，以及更有效的使用 Vue 来加速开发我们的应用。有报告指出，我们花费 70%的时间阅读代码，30%的时间编写代码，作为个人而言，身为维护者的我，喜欢这种感觉，通过查看代码库，开启了我之前从未了解过的编写方法，并且马上了解作者在`methods`，`computed`，`watchers`的区别用意。

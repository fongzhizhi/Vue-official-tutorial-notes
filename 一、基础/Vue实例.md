## 创建一个Vue实例

> 每个Vue应用都是通过`Vue`函数创建的实例开始的。虽然没有完全遵循 [MVVM 模型](https://zh.wikipedia.org/wiki/MVVM)，但是 Vue 的设计也受到了它的启发。

当创建一个 Vue 实例时，你可以传入一个**选项对象**。这篇教程主要描述的就是如何使用这些选项来创建你想要的行为。作为参考，你也可以在 [API 文档](https://cn.vuejs.org/v2/api/#选项-数据)中浏览完整的选项列表。

一个 Vue 应用由一个通过 `new Vue` 创建的**根 Vue 实例**，以及可选的嵌套的、可复用的组件树组成。举个例子，一个 todo 应用的组件树可以是这样的：

```txt
根实例
└─ TOdoList
	|-- TodoItem
		|-- TodoButtonDelete
		|-- TodoButtonEdit
	|-- TodoListFooter
		|-- TodosBUttonClear
		|-- TodoListStatistics
```

我们会在稍后的[组件系统](https://cn.vuejs.org/v2/guide/components.html)章节具体展开。不过现在，你只需要明白**所有的 Vue 组件都是 Vue 实例**，并且接受相同的选项对象 (一些根实例特有的选项除外)。

## 数据与方法

当一个Vue实例被创建，它将会把`data`对象中的所有property加入到Vue的**响应式系统**中。当这些值发生变化时，将会同步更新视图上的变化。

此外，`data`对象对上的property也会被绑定到Vue实例上：

```js
// 数据对象
const data = {
    name: 'Jinx',
    age: 23,
}

const vm = new Vue({
    el: '#app',
    data, // 绑定到Vue实例上
})

vm.name === data.name // true
```

> 值得注意的是，如果数据对象使用`Object.freeze`之后，数据将不能实现响应效果。

除了数据property会绑定到实例对象上，还会额外暴露一些有用的数据或方法，这些方法都会带有`$`前缀以便和用户自定义的property区分开来。

```js
const data = {}
const vm = new Vue({
    el: '#app',
    data,
})

vm.$data === data // true
vm.$el === document.getElementById('app') // true
// 监听数据vm.age的变化处理函数
vm.$watch('age', function(newValue, oldvalue) {
    //vm.age变化后的回调
})
```

更多实例property请参考[官方api](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B-property)。

## 实例的生命周期钩子

一个Vue实例的创建需要经过一系列初始化过程才能实现数据监听、模板编译、将实例挂载到DOM并在数据变化时更新DOM等功能。所以Vue在创建实例的这一系列过程中挂载了阶段性的**生命周期钩子函数**，以便用户能在不同阶段加入自定义的代码。

比如`created`钩子可以用来在实例创建之后执行相应的逻辑代码：

```js
new Vue({
    el: '#app',
    data: {
        name: 'Jinx',
        age: 24,
    },
    created: function(){
    	// this 指向当前实例对象
    	this.age++
	},
})
```

> 值得注意的是，生命周期函数的`this`上下文将指向当前实例对象，所以不要在实例property或回调函数上使用箭头函数，这常常导致`Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错。

## 生命周期图示

除了，上面提到的`created`钩子，Vue实例生命周期钩子如下图所示，在未深入使用实例之前，对这些钩子函数只需做简单了解即可，但随着你的学习和使用，生命周期图的参考价值会越来越高：

![Vue 实例生命周期](../../MarkDown-Nodes/images/lifecycle.png)

可以发现，一个Vue实例大致需要进过：相关事件和对象的初始化、`created`、`mounted`、`estroyed`几个阶段。

而`updated`是在`mounted`时初始化的，当监听的数据发生变化时才会触发。
内联class和内联样式的绑定是十分常见的使用场景，因为class和样式属性style都是原生的html attribute，所以使用`v-bind`指令就可以实现class和style的数据绑定。但class和style属性与别的属性有很大的差异，所以Vue**专门针对class属性和style属性进行了v-bind指令增强**：由于一般的字符串绑定容易造成错误拼接等问题，所以在绑定class和style时可以使用更为直观的**对象语法**或**数组语法**。

## Class的绑定

### 对象语法

绑定类时，我们可以传递一个对象字符串给v-bind：

```html
<div id="app">
    <p :class="{actice: isActice, red: isRed}">{{ message }}</p>
</div>
```

```js
new Vue({
    el: '#app',
    data: {
    	isActive: true,
        isRed: false,
    }
})
```

这样，绑定的class对象就会将值为`truthy`的键渲染为该标签的class。

为了更直观地观察类的变化，我们一般使用**计算属性**来绑定类名：

```html
<div id="app">
    <p :class="pClass">{{ message }}</p>
</div>
```

```js
new Vue({
    el: '#app',
    data: {
    	isActive: true,
        isRed: false,
    },
    computed: {
        pClass: function(){
            return {
                active: this.isActive,
                red: this.isRed,
            }
        }
    }
})
```

### 数组语法

我们知道，`classList`本身就是一个类数组，所以Vue也提供了数组语法进行class的绑定，用法也很简单：

```html
<p :class="[active, red]"></p>
```

```js
new Vue({
    data: {
        active: 'active',
        red: 'red',
        isRed: true,
    }
})
```

此外，数组类语法中同样**能嵌套使用对象语法**：

```html
<p :class="[active, {red: isRed}]"></p>
```

或者使用三元表达式：

```html
<p :class="[active, isRed ? red : '']"></p>
```

> 值得注意的是，当我们动态绑定class时，还可以联合使用原生的class属性，Vue将会在原生属性上属性上扩展添加，不过建议还是写到一起，方便观察。
>
> ```html
> <p class="big" :class="[actice, red]"></p>
> ```

### 组件内的使用

在组件内使用是一样的效果，我们只需要在组件标签上正常使用类绑定语法即可，动态绑定的类名会自动增加到模板中：

```js
Vue.component('my-temp', {
    template: '<p class="big red">Hello</p>'
})
```

```html
<my-temp :class="[active]"></my-temp>
```

这样，最终的渲染结果为：

```html
<p class="big red active">Hello</p>
```

## 绑定内联样式

内联样式的绑定与类的绑定无太大区别。

### 对象语法

同样的，也可以使用计算属性。

 ```html
<div id="app">
    <p :style="pStyle">THis is a message</p>
</div>
 ```

```js
new Vue({
    el: '#app',
    data: {
        color: '#123aaa',
        fontsize: '16px'
    },
    computed: {
        pStyle: function(){
            return {
                color: this.color,
                fontSize: this.fontSize,
            }
        }
    }
})
```

### 数组语法

`v-bind:style` 的数组语法可以将多个**样式对象**应用到同一个元素上：

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### 自动添加前缀

当 `v-bind:style` 使用需要添加[浏览器引擎前缀](https://developer.mozilla.org/zh-CN/docs/Glossary/Vendor_Prefix)的 CSS property 时，如 `transform`，Vue.js 会自动侦测并添加相应的前缀。

### 多重值

从 2.3.0 起你可以为 `style` 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 `display: flex`。
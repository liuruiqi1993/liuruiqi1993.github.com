# Vue笔记

遇到的几个vue的问题，答不上来

1. 源码
2. 虚拟dom
3. vue.delete
4. provide/inject
5. $attr, $listener
6. transition，transition-group

重新看了文档之后的问题

1. [passive修饰符](https://www.cnblogs.com/ziyunfei/p/5545439.html)
    浏览器无法预先知道一个监听器会不会调用 `preventDefault()`，它能做的只有等监听器执行完后再去执行默认行为，导致页面卡顿。passive 的意思是“顺从的”，表示它不会对事件的默认行为说 no，浏览器知道了一个监听器是 passive 的，它就可以在两个线程里同时执行监听器中的 JavaScript 代码和浏览器的默认行为了。 passive 监听器能保证的只有一点，那就是调用 `preventDefault()` 无效.
2. 想起从前项目中的一个bug，element-ui表格按字母表分类，array变化后显示有问题。
3. Vue.config.optionMergeStrategies

## 响应式原理

* 把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的属性，并使用 `Object.defineProperty` 把这些属性全部转为 `getter/setter`。
* 每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据属性记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。
* getter/setter让 Vue 能够追踪依赖，在属性被访问和修改时通知变更。
* Vue 在更新 DOM 时是异步执行的。如果同一个 watcher 被多次触发，会去重再将实际操作放入队列中，等待DOM刷新。所以如果想操作数据刷新后的DOM应该在nextTick中进行。
* nextTick返回Promise对象，所以可以`await this.$nextTick()`

## vue 实例

* 只有当实例被创建时就已经存在于 data 中的属性才是响应式的。如果你知道你会在晚些时候需要一个属性，那么你仅需要设置一些初始值。
* 不要在选项属性或回调上使用箭头函数，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`，因为箭头函数并没有 this，this 会作为变量一直向上级词法作用域查找，直至找到为止。
* 不需要关注变化就`Object.freeze()`。

## 模板语法

Vue 将模板编译成 虚拟DOM 渲染函数，计算出最少需要重新渲染多少组件。

* 每个绑定都只能包含单个表达式，所以`if (ok) { return message }` `var a = 1`都不会生效。模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 `Math` 和 `Date` 。你不应该在模板表达式中试图访问用户定义的全局变量。
* `v-html`只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值，容易导致 XSS 攻击。
* `<a v-bind:[attributeName]="url"> ... </a>`
中括号内不可以有空格引号，动态`attributeName`考虑用Computed实现
`attributeName`预期`String`，异常时为`null`，其他情况触发警告

## 计算属性和侦听器

* Computed是基于它们的响应式依赖进行缓存的。只在**相关响应式依赖发生改变**时它们才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。
* Methods总会再次执行函数。
* Watch是Computed备选，Computed解决不了时再watch，比如：当需要在数据变化时执行异步或开销较大的操作时。

## Class 与 Style 绑定

可以放`String`, `Array`, `Object`

```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div> //这样写只会渲染数组中最后一个被浏览器支持的值。
```

## v-if, v-show, v-once条件渲染

* 添加`key`是为了让两个元素独立，不复用。
* `v-show` 不支持 `<template>` 元素， `v-if`有销毁重建，`v-show`仅仅是隐藏显示。
* 当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。最好是直接将列表过滤之后再循环。

## v-for列表渲染

* 遍历对象时`{value, key, index}`第三个参数是index。
* **不要使用对象或数组之类的非基本类型值**作为 v-for 的 `key` 。请用字符串或数值类型的值。
* 过滤`<li v-for="n in even(numbers)">{{ n }}</li>` even是个function。

**Vue 不能检测以下数组的变动：**

* 当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`

```js
vm.$set(vm.items, indexOfItem, newValue)
vm.items.splice(indexOfItem, 1, newValue)
vm.userProfile = Object.assign({}, vm.userProfile, obj)
```

* 当你修改数组的长度时，例如：`vm.items.length = newLength`

```js

vm.items.splice(newLength)
// 我还以为可以直接把数组变长，然而其实并不行
// splice(start, deleteCount, item1, item2...)
// deleteCount省略，start之后数组的所有元素都会被删除。
// start超出长度向末尾添加，负数从末尾删除。

let arr = [1,2,3,4,5,6]
arr.splice(8)
console.log(arr) //[1, 2, 3, 4, 5, 6]

let arr = [1,2,3,4,5,6]
arr.splice(-8)
console.log(arr) //[]

let arr = [1,2,3,4,5,6]
arr.splice(3)
console.log(arr) //[1, 2, 3]
```

## v-on:event事件处理

有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法：

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

## v-model表单输入绑定

* `v-model` 会忽略所有表单元素的 value、checked、selected attribute 的初始值而总是将 Vue 实例的数据作为数据来源。
* 自定义`v-model`

```
model: {
  prop: 'checked',
  event: 'change'
}
```

* text 和 textarea 元素使用 value 属性和 input 事件；input和change的区别是会影响中文输入。
checkbox 和 radio 使用 checked 属性和 change 事件；
select 字段将 value 作为 prop 并将 change 作为事件。
* number 修饰符返回number类型

## 组件基础

一个组件的 **data 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝。

* 为了满足元素嵌套规则可以写成

```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```

## 组件注册(同理router引入全部文件)

### 组件和路由自动化

[https://www.daozhao.com/8605.html](https://www.daozhao.com/8605.html)

```js
// require.context(directory, useSubdirectories = true, regExp = /^\.\/.*$/, mode = 'sync');
// mode可选"sync" | "eager" | "weak" | "async-weak" | "lazy" | "lazy-once"

// sync || eager:      just add all dependencies and continue
// lazy-once:          create a new async dependency block and add that block to this context
// lazy:               a new async dependency block per dependency and add all blocks to this context
// weak || async-weak: we mark all dependencies as weak

const requireComponent = require.context(
  './components',              // 其组件目录的相对路径
  false,                       // 是否查询其子目录
  /Base[A-Z]\w+\.(vue|js)$/    // 匹配基础组件文件名的正则表达式
)
requireComponent.keys().forEach(fileName => {
  console.log(requireComponent(fileName))
})
```

## Prop

* 验证

```js
prop: {
    type: '字符串或数组',
    validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
    }
}
// prop 会在一个组件实例创建之前进行验证，所以实例的属性 (如 data、computed 等) 在 default 或 validator 函数中是不可用的。
```

* 传入所有属性`<blog-post v-bind="post"></blog-post>`等价于

```html
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

* 对于绝大多数 attribute 来说，从外部提供给组件的值会替换掉组件内部设置好的值。所以如果传入 `type="text"` 就会替换掉 `type="date"` 并把它破坏！庆幸的是，class 和 style
* 有了 `inheritAttrs: false` 和 `$attrs`，你就可以**手动决定这些 attribute 会被赋予哪个元素**。这个模式允许你在使用基础组件的时候更像是使用原始的 HTML 元素，而不会担心哪个元素是真正的根元素。

## 自定义事件

```js
model: {
    prop: 'checked' //默认value,
    event: 'change' //默认input
}
```

* `$listeners`里面包含了作用在这个组件上的所有监听器。通过`Computed + Object.assign({}, this.$listener) + v-on`可以重写事件。
* 注意带有 .sync 修饰符的 v-bind 不能和表达式一起使用 (例如 `v-bind:title.sync=”doc.title + ‘!’”` 是无效的)。取而代之的是，你只能提供你想要绑定的属性名，类似 v-model。
* `<text-document v-bind.sync="doc"></text-document>`这样会把 doc 对象中的每一个属性 (如 title) 都作为一个**独立的 prop 传进去，然后各自添加用于更新的 v-on 监听器**。

## 插槽

* 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

* `<slot>Submit</slot>`Submit就是默认的`placeholder`。

* 注意 `v-slot` 只能添加在 <template> 上 (只有一种例外情况)，这一点和已经废弃的 slot attribute 不同。

* 作用域插槽用法，解构

```vue
<current-user>
  <template v-slot:other="otherSlotProps">
    ...
  </template>

  // 解构
  <template v-slot:default="{ user: person }">
    {{ person.firstName }}
  </template>

  // 默认值
  <template v-slot:another="{ user = { firstName: 'Guest' } }">
    {{ user.firstName }}
  </template>
</current-user>
```

## 动态组件 & 异步组件

### keep-alive

* 注意这个 <keep-alive> 要求被切换到的组件都有自己的名字，不论是通过组件的 name 选项还是局部/全局注册。

### 异步组件

```js
components: {
    a: require(['./my-async-component'], resolve),
    b: () => import('./my-async-component'),
    c: () => ({
        // 需要加载的组件 (应该是一个 `Promise` 对象)
        component: import('./MyComponent.vue'),
        // 异步组件加载时使用的组件
        loading: LoadingComponent,
        // 加载失败时使用的组件
        error: ErrorComponent,
        // 展示加载时组件的延时时间。默认值是 200 (毫秒)
        delay: 200,
        // 如果提供了超时时间且组件加载也超时了，
        // 则使用加载失败时使用的组件。默认值是：`Infinity`
        timeout: 3000
    })
}
```

## 处理边界情况

应该避免在模板或计算属性中访问 `$refs`。

### 依赖注入 provide 和 inject

**provide中的属性和方法可以被后代访问到。**

### 循环(组件是调用自身)

1. 等到beforeCreate载绑上去

```js
beforeCreate: function () {
  this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue').default
}
```

2. 异步

```js
components: {
  TreeFolderContents: () => import('./tree-folder-contents.vue')
}
```

### 程序化时间侦听`hook:beforeDestroy`

```js
mounted: function () {
  this.attachDatepicker('startDateInput')
  this.attachDatepicker('endDateInput')
},
methods: {
  attachDatepicker: function (refName) {
    var picker = new Pikaday({
      field: this.$refs[refName],
      format: 'YYYY-MM-DD'
    })

    this.$once('hook:beforeDestroy', function () {
      picker.destroy()
    })
  }
}
```

## 过渡 & 动画

过渡和动画是两个事件，用type（可选值animation||transition）来确定监听transitionend和animationend

### 钩子

```html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```

## mixin混入

和组件都是复用，但区别是`this`，可以用在比如发送短信60s。

## 渲染函数 &JSX

新建[[h1-6]]标签时，省去`v-if`判断`this.level`

```js
render: function (createElement) {
    return createElement(
      'h' + this.level,   // 标签名称
      this.$slots.default // 子节点数组
    )
}
```

### 虚拟DOM，JSX，函数式组件

看不懂

## API

### config

#### [optionMergeStrategies](https://cn.vuejs.org/v2/api/#optionMergeStrategies)

#### [keyCodes](https://cn.vuejs.org/v2/api/#keyCodes)

#### [Vue.set](https://cn.vuejs.org/v2/api/#Vue-set)和[Vue.delete](https://cn.vuejs.org/v2/api/#Vue-delete)

还是不懂为什么要删掉，我感觉日常把它设为空就好了，或许是用在数组套对象的时候，没用过，先记下来。

### data

如果需要，可以通过将 `vm.$data` 传入 `JSON.parse(JSON.stringify(...))` 得到深拷贝的原始数据对象。

### provide / inject

`provide` 和 `inject` 绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。
用`Vue.observable`可以使其响应式

```
const state = Vue.observable({ count: 0 })

const Demo = {
  render(h) {
    return h('button', {
      on: { click: () => { state.count++ }}
    }, `count is: ${state.count}`)
  }
}
```

### [watch](https://cn.vuejs.org/v2/api/#vm-watch)

停止监听，但是`immediate: true`的第一次无法取消监听。

```js
var unwatch = vm.$watch(
  'value',
  function () {
    doSomething()
    if (unwatch) {
      unwatch()
    }
  },
  { immediate: true }
)
```

### forceUpdate

迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。

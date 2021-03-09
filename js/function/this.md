# this指向

参考：[浅析this--this绑定规则及优先级](https://juejin.cn/post/6925082635442225160)

## 指向：调用者

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25, 正常结果
window.getAge(); // NaN  因为 y - undefined

var fn = xiaoming.age; // 先拿到xiaoming的age函数
window.fn(); // NaN this还是window
```

`this`指针只在`age`方法的函数内指向`xiaoming`，在函数内部定义的函数，`this`又指向`undefined`了！（在非`strict`模式下，它指向全局对象`window`，`strict`中为`undefined`，因为`strict`不允许未声明的变量）。

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```

## apply\(\)， bind\(\)，call\(\)

`call()`、`apply()`、`bind()` 都是用来重定义 `this` 这个对象的。 `bind` 返回的是一个新的函数，你必须调用它才会被执行。

```javascript
obj.myFun.call(db,'成都','上海')；   // 德玛 年龄 99 来自 成都去往上海
obj.myFun.apply(db,['成都','上海']); // 德玛 年龄 99 来自 成都去往上海
obj.myFun.bind(db,'成都','上海')();  // 德玛 年龄 99 来自 成都去往上海
obj.myFun.bind(db,['成都','上海'])();// 德玛 年龄 99 来自 成都, 上海去往 undefined
```

## new绑定&gt;显式绑定&gt;隐式绑定&gt;默认绑定 

同时我们可以得出一套规律：

* 函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。
* 函数是否通过 call、apply、bind（显式绑定）或者**硬绑定**调用？如果是的话，`this` 绑定的是指定的对象。
* 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，`this` 绑定的是那个上下文对象。
* 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 `undefined`，否则绑定到全局对象。

### 硬绑定

硬绑定属于显式绑定的一个变种，但是却可以解决这个问题。在函数 `bar()`中，我们调用了 `foo.call(obj)`，这样会强制把 `foo` 的 `this` 绑定到了 `obj`。无论之后如何调用函数 `bar`，`this`都会再次绑定到`obj`。这种绑定是一种显式的强制绑定，因此我们称之为硬绑定。

```javascript
function foo() {console.log( this.a ); }
var obj = { a:2 };
var bar = function() { foo.call( obj ); };
bar(); // 2
setTimeout( bar, 100 ); // 2

// 硬绑定的 bar 不可能再修改它的 this bar.call( window ); // 2
```

### API调用上下文

第三方库的许多函数，以及 JS语言和环境中许多新的内置函数，都提供了一个可选的参数，通常被称为“上下文”，其作用和 `bind(..)` 一样，确保你的回调函数使用指定的 `this`。

```javascript
function foo(id) { console.log( id, this.id ); }
var obj = { id: "dmeo" };
// 调用 foo(..) 时把 this 绑定到 obj
[1, 2, 3].forEach( foo, obj ); // 1 demo 2 demo3 demo
```


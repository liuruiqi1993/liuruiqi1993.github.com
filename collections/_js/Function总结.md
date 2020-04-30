---
layout: post
title: Function总结
date: 2020-04-28
---

## 基础
* 作用域：
  * 函数体
  * 全局作用域只有一个。
* 语法`new Function ([arg1[, arg2[, ...argN]],] functionBody)`
* `arguments` 是一个对应于传递给函数的参数的类数组对象，最常用于判断传入参数的个数。。
  * `arguments.callee` 指向当前执行的函数。
  * `arguments.length` 指向传递给当前函数的参数数量。
* `Function.caller` 获取调用函数的具体对象。该特性是非标准的，请尽量不要在生产环境中使用它！
* `Function.length` 获取函数的接收参数个数。
* `Function.name` 获取函数的名称。

### 变量提升
提升声明但不会提升赋值。

### 函数提升
```
//函数声明式，提到作用域最前面
function bar () {}

//函数字面量式 与变量提升一致，定义后才能console，否则undefined
var foo = function () {}
```

### this
#### 指向
* 被调用的对象。

```
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
`this`指针只在age方法的函数内指向`xiaoming`，在函数内部定义的函数，`this`又指向`undefined`了！（在非`strict`模式下，它重新指向全局对象`window`！）
```
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


#### apply()， bind()，call()
`call()`、`apply()`、`bind()` 都是用来重定义 `this` 这个对象的。  
`bind` 返回的是一个新的函数，你必须调用它才会被执行。
```
obj.myFun.call(db,'成都','上海')；   // 德玛 年龄 99 来自 成都去往上海
obj.myFun.apply(db,['成都','上海']); // 德玛 年龄 99 来自 成都去往上海  
obj.myFun.bind(db,'成都','上海')();  // 德玛 年龄 99 来自 成都去往上海
obj.myFun.bind(db,['成都','上海'])();// 德玛 年龄 99 来自 成都, 上海去往 undefined
```

### 箭头函数
箭头函数内部的this是词法作用域，由上下文确定。
用`call()`或者`apply()`调用箭头函数时，无法对`this`进行绑定，即传入的第一个参数被忽略。

### 剩余参数
如果函数的最后一个命名参数以...为前缀，则它将成为一个由剩余参数组成的真数组。
```
function(a, b, ...theArgs) {
  // ...
}
```
**剩余参数和 arguments对象的区别**
* 剩余参数只包含那些没有对应形参的实参，而 arguments 对象包含了传给函数的所有实参。
* `arguments`对象不是一个真正的数组，而剩余参数是真正的`Array`实例。
* `arguments`对象还有一些附加的属性（如`callee`属性）。

## 闭包
闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来。  
函数1返回一个函数2，通过调用函数2可以访问函数1里的变量。
```
// 定义数字0:
var zero = function (f) {
    return function (x) {
        return x;
    }
};
// zero(f)(x) = x

// 定义数字1:
var one = function (f) {
    return function (x) {
        return f(x);
    }
};
// one(f)(x) = f(x)

// 定义加法: m,n换过来也没有关系
function add(n, m) {
    return function (f) {
        return function (x) {
            return m(f)(n(f)(x));
        }
    }
}
// add(n,m)(f)(x) = m(f)(n(f)(x))

// 计算数字2 = 1 + 1:
var two = add(one, one);
// two(f)(x) = one(f)(one(f)(x))
       = one(f)(f(x))
       = f(f(x))

// 计算数字3 = 1 + 2:
var three = add(one, two);
// three(f)(x) = two(f)(one(f)(x))
               = f(f(one(f)(x)))
               = f(f(f(x)))

// 计算数字5 = 2 + 3:
var five = add(two, three);
// five(f)(x) = three(f)(two(f)(x))
              = f(f(f(two(f)(x))))
              = f(f(f(f(f(x)))))

five = add(three, two)
// five(f)(x) = two(f)(three(f)(x))
              = f(f(three(f)(x)))
              = f(f(f(f(f(x)))))
```

## 构造函数
用new调用，返回一个对象的函数。

```
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}
```

### new一个对象具体做了什么
1. 当以new关键字调用时，会创建一个新的内存空间，标记为 Animal 的实例。
2. 函数体内部的 this 指向该内存
3. 执行函数体内的代码，给this添加属性和方法
4. 默认返回this
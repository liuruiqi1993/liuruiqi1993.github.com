# Function总结

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

```javascript
//函数声明式，提到作用域最前面
function bar () {}

//函数字面量式 与变量提升一致，定义后才能console，否则undefined
var foo = function () {}
```

### 箭头函数

箭头函数内部的`this`是词法作用域，由上下文确定。 用`call()`或者`apply()`调用箭头函数时，无法对`this`进行绑定，即传入的第一个参数被忽略。

### 剩余参数

如果函数的最后一个命名参数以...为前缀，则它将成为一个由剩余参数组成的真数组。

```javascript
function(a, b, ...theArgs) {
  // ...
}
```

#### **剩余参数和 arguments对象的区别**

* 剩余参数只包含那些没有对应形参的实参，而 arguments 对象包含了传给函数的所有实参。
* `arguments`对象不是一个真正的数组，而剩余参数是真正的`Array`实例。
* `arguments`对象还有一些附加的属性（如`callee`属性）。

## 构造函数

用new调用，返回一个对象的函数。

```javascript
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}
```

### new一个对象具体做了什么

1. 当以new关键字调用时，会创建一个新的内存空间，标记为 Animal 的实例。
2. 函数体内部的 this 指向该内存
3. 执行函数体内的代码，给this添加属性和方法
4. 默认返回this


# 闭包

## 概念

闭包是指有权访问另一个函数**作用域**中的变量的函数。

常见形式是在函数a内返回一个函数b，函数b既可以访问a外面的变量，也可以访问a中定义的变量。

## 特点
!(JavaScript作用域和闭包理解总结)[https://juejin.cn/post/6944609078186344484#heading-9]
- 在后台执行环境中，闭包的作用域链包含着它自己的作用域、包含函数的作用域和全局作用域。
- 通常，函数的作用域及其所有变量都会在函数执行结束后被销毁。
- 但是，当函数返回了一个闭包(函数)时，这个函数的作用域将会一直在内存中保存到闭包不存在为止。这里解释一下为什么函数的作用域及其所有变量都会在函数执行结束后被销毁，因为函数执行实际上是一个栈式结构的执行顺序，我们称为执行栈，先调用的函数先入栈，后出栈，如下函数：

```js
function a(){
  console.log('a入栈，开始执行')
  //......
  b()
  console.log('a执行结束，出栈')
}
function b(){
  console.log('b入栈，开始执行')
  //......
  c()
  console.log('b执行结束，出栈')
}
function c(){
  console.log('c入栈，开始执行')
  //......
  console.log('c执行结束，出栈')
}
a()
```
我们在代码的最后执行a()函数，a函数入栈，a函数中调用b()使得b入栈，b函数中调用c()使得c入栈;接下来c函数执行完成出栈，b函数执行完成出栈，最后栈顶的a出栈执行完成。因此外部函数执行完成时，内部函数都已经出栈，所以可以释放内存。

但是当存在闭包时情况会有所不同，因为外部存在内部函数的引用时，因此即使执行栈都已清空，也不能回收外部函数的作变量对象所占用的内存，这就形成了闭包。形如下面的函数，
```js
function createPlus(){
    var num=1;
    function plus(){
       num+=1;
       return num;
    }
    return plus;
}
let plus=createPlus();
plus();
```
代码中plus函数作为createPlus函数的内部函数，它可以访问自己的作用域createPlus函数的作用域和全局作用域，我们在createPlus函数中把plus函数作为值返回，并且赋值给一个外部变量plus，这就形成了一个闭包，导致createPlus函数的作用域无法释放，除非外部的函数引用变量置空即plus=null，否则刺闭包内的变量不会被释放内存。

上面的代码只是一种闭包的形式，实际上**任何把函数当做值来传递的场景中都会触发闭包的形成**，比如我们最常用的回调函数的形式:

```js
var message='hello'
function update(){
  message='Helllo'
}
setTimeout(update)
window.addEventListnener('click',update);
```
这也是我们需要及时remove移除事件监听程序的原因，因为我们把事件处理函数当做值来传递，实际上是创建了对函数的引用形成了闭包，这个事件处理函数会包含外部环境作用域的引用，导致外部作用域内存无法释放。

**优点**：可以重复使用变量，并且不会造成变量污染，可以用来定义私有属性和私有方法。

**缺点**：比普通函数更占用内存，会导致网页性能变差，在 IE 下容易造成内存泄露。

```javascript
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
```

## 应用

### 函数防抖

比如使用搜索框，为了避免频繁发请求，定义一个定时器，一旦在规定时间内重新输入就重新计时。

### 函数节流

比如`resize`窗口，定义一定时间内只执行一次，其余`return`，或者上拉加载，在上一个请求结束之前，return其余的加载请求

### 柯里化

分次传入多个参数直至参数足够执行函数。

**优点**：

* 参数复用：减少重复传递不变的部分参数
* 延迟计算：节流 防抖
* 提前确认

```javascript
const curryingFun = (fn, arr = []) => (...args) => (
    (arg) => (
        arg.length === fn.length 
            ? fn(...arg) 
            : curryingFun(fn, arg)
    )
)([ ...arr, ...args ]);
```

### (模块化)[../../../webpack.md]






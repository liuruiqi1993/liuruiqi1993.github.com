# 闭包

## 概念

闭包是指有权访问另一个函数**作用域**中的变量的函数。

常见形式是在函数a内返回一个函数b，函数b既可以访问a外面的变量，也可以访问a中定义的变量。

## 特点

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








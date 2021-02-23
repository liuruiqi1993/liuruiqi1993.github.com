# async, await, generator

一个generator看上去像一个函数，但可以返回多次。调用generator对象有两个方法：

```javascript
function* foo(x) {
    yield x + 1;
    yield x + 2;
    return x + 3;
}
```

* 用`for ... of`循环迭代generator对象
* 调用generator对象的`next()`方法，返回一个对象`{value: x, done: true/false}`，`value`就是`yield`的返回值，`done`表示这个generator是否已经执行结束了。**下一次调用next的时候，传的参数会被作为上一个yield前面接受的值**

```javascript
const getData = () => new Promise(resolve => setTimeout(() => resolve("data"), 1000))
function* testG() {
  // await被编译成了yield
  const data = yield getData()
  console.log('data: ', data);
  const data2 = yield getData()
  console.log('data2: ', data2);
  return 'success'
}
var gen = testG()
gen.next('这个参数才会被赋给data变量')
gen.next('666')

// data:  666
// {value: Promise, done: false}
```

## async await

[https://juejin.im/post/5e79e841f265da5726612b6e](https://juejin.im/post/5e79e841f265da5726612b6e)


# 异步加载

## 概念

在JavaScript的世界中，所有代码都是单线程执行的，导致JavaScript的所有网络操作，浏览器事件，都必须是异步执行。异步执行可以用回调函数实现。**异步操作会在将来的某个时间点触发一个函数调用。**

```text
function test(resolve, reject) {
    var timeOut = Math.random() * 2;
    log('set timeout to: ' + timeOut + ' seconds.');
    setTimeout(function () {
        if (timeOut < 1) {
            log('call resolve()...');
            resolve('200 OK');
        }
        else {
            log('call reject()...');
            reject('timeout in ' + timeOut + ' seconds.');
        }
    }, timeOut * 1000);
}
```

[https://www.cnblogs.com/xiaojianwei/p/12209471.html](https://www.cnblogs.com/xiaojianwei/p/12209471.html)

## Promise

变量`p1`是一个Promise对象，它负责执行`test`函数。由于`test`函数在内部是异步执行的，当`test`函数执行完毕，`p1`就能调用`then`和`catch`

```text
var p1 = new Promise(test);

job1.then(job2).then(job3).catch(handleError); // job1、job2和job3都是Promise对象

Promise.race([p1, p2]).then(function (result) {});
Promise.all([p1, p2]).then(function (result) {});
```


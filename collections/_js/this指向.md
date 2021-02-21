---
layout: post
title: this指向
date: 2021-02-18
---

https://juejin.cn/post/6925082635442225160

new绑定>显式绑定>隐式绑定>默认绑定
同时我们可以得出一套规律：

* 函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。
* 函数是否通过 call、apply、bind（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是指定的对象。
* 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上下文对象。
* 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定到全局对象。

1.硬绑定
       硬绑定属于显式绑定的一个变种，但是却可以解决这个问题。在函数 bar()中，我们调用了 foo.call(obj)，这样会强制把 foo 的 this 绑定到了 obj。无论之后如何调用函数 bar，this都会再次绑定到obj。这种绑定是一种显式的强制绑定，因此我们称之为硬绑定。 

```
function foo() {console.log( this.a ); }
var obj = { a:2 };
var bar = function() { foo.call( obj ); };
bar(); // 2 
setTimeout( bar, 100 ); // 2 

// 硬绑定的 bar 不可能再修改它的 this bar.call( window ); // 2

```

2.API调用上下文
第三方库的许多函数，以及 JS语言和环境中许多新的内置函数，都提供了一个可选的参数，通常被称为“上下文”，其作用和 bind(..) 一样，确保你的回调函数使用指定的 this。

```
function foo(id) { console.log( id, this.id ); }
var obj = { id: "dmeo" };
// 调用 foo(..) 时把 this 绑定到 obj 
[1, 2, 3].forEach( foo, obj ); // 1 demo 2 demo3 demo
```

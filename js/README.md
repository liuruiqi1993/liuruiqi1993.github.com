# Javascript

## 基础概念

### 强弱类型语言的定义

js是弱类型语言，因为同一个变量可以**反复赋值**，而且可以是**不同类型**的变量。  
而强类型定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。

### strict严格模式

强制要求用`var`申明变量。没有通过`var`申明就被使用，那么该变量就自动被申明为全局变量。

### Truthy和Falsy

`null`、`undefined`、`0`、`NaN`和空字符串`''`视为false，其他值一概视为true。

### 循环

#### for

* `break` 结束当前层级的循环，外层继续。
* `continue` 中止本次循环，接着开始下一次循环。 
* `return` 是用来结束函数的

如果`for`循环没有写在`function`中会报`Illegal return statement`。

#### for...key...in

#### for...item...of

#### while \(\) {}

每次循环开始的时候判断条件。

#### do {} while \(\)

每次循环完成的时候判断条件,至少执行一次。

## 简写

### 不确定

* `const map = list.reduce((res, v) => (res[v.id] = v, res), {})`  逗号是什么意思?`return res`

### 总结

* `a && b()`
* `a = b || c`
* [解构赋值: 数组 + 对象 + 字符串](./#解构赋值)
  * `[x, y] = [y, x]`交换x，y的值
* [扩展运算符`...`用于数组或对象浅拷贝，函数调用](./#扩展运算符)
* 三元运算
* `obj == null` 可以用来检查 `null || undefined`

### 解构赋值

可以将属性/值从对象/数组中取出,赋值给其他变量。

```javascript
let {a,b,c,...rest} = {a: 1,b: 2,c: 3,d: 4,e: 5}
console.log(a,b,c,rest) // 1 2 3 {d: 4, e: 5}

let [a,b,c,...rest] = [1,2,3,4,5]
console.log(a,b,c,rest) // 1 2 3 [4, 5]

let [a,b,c,...rest] = '12345'
console.log(a,b,c,rest) // "1" "2" "3" ["4", "5"]

----------有问题分割线----------

let {a,b,c,...rest} = '12345'
console.log(a,b,c,rest) // undefined undefined undefined {0: "1", 1: "2", 2: "3", 3: "4", 4: "5"}

let {a,b,c,...rest} = 12345
console.log(a,b,c,rest) // undefined undefined undefined {}

let [a,b,c,...rest] = 12345
console.log(a,b,c,rest) // error: 12345 is not iterable
```

### 扩展运算

可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开；还可以在构造字面量对象时, 将对象表达式按key-value的方式展开。的方式展开。

#### **浅拷贝** 

可替代`Array.unshift`，`Array.concat`，`Object.assign`

```javascript
let obj = {a: 1, b: 2, c: {d: 4}, e: [5]}
let objClone = { g: 7, ...obj, f: 6 };
obj.b = 20
obj.c.d = 40
obj.e[0] = 50
console.log(objClone) 
// {g: 7, a: 1, b: 2, c: {d: 40}, e: [50], f: 6}
// 一层是深复制，多层是浅复制

let arr = [1, 2, {a: 3}, [4]]
let arrClone = [ 7, ...arr, 5 ];
arr[1] = 20
arr[2].a = 30
arr[3][0] = 40
console.log(arrClone) // [7, 1, 2, {a: 30}, [40], 5]
```

#### **函数调用**

```javascript
//等价于apply的方式
function myFunction(v, w, x, y, z) { }
var args = [0, 1];
myFunction(-1, ...args, 2, ...[3]);

//在 new 表达式中应用
var dateFields = [1970, 0, 1]; // 1970年1月1日
var d = new Date(...dateFields);
```

### [正则表达式预查](https://www.cnblogs.com/allen2333/p/9835654.html)

```javascript
 export default function (source, length = 3) {
  source = String(source).split(".");
  source[0] = source[0].replace(new RegExp('(\\d)(?=(\\d{'+length+'})+$)','ig'),"$1,");
  return source.join(".");
}
```

`\d`是把`\d`当作一个向后引用，先占位， 对一个正则表达式模式或部分模式两边添加圆括号将导致相关匹配存储到一个临时缓冲区中，所捕获的每个子匹配都按照在正则表达式模式中从左到右出现的顺序存储。 可以使用非捕获元字符 `?:`、`?=` 或 `?!` 来重写捕获，忽略对相关匹配的保存。

```javascript
console.log(('100000000').replace(new RegExp('(\\d)((\\d{3})+$)','ig'),"$1,$2     "))
100,000000
```

`(?=exp)`也叫零宽度正预测先行断言，它断言自身出现的位置的后面能匹配表达式exp。比如`\b\w+(?=ing\b)`，匹配以ing结尾的单词的前面部分\(除了ing以外的部分\)，如查找I'm singing while you're dancing.时，它会匹配sing和danc。 `(?<=exp)`也叫零宽度正回顾后发断言，它断言自身出现的位置的前面能匹配表达式exp。比如`(?<=\bre)\w+\b`会匹配以re开头的单词的后半部分\(除了re以外的部分\)，例如在查找reading a book时，它匹配ading。

`$1`匹配的是`(\d)`,`$2`匹配`(?=(\d{'+length+'})+$)`

```javascript
console.log(('1000000000').replace(new RegExp('(\\d)(?=(\\d{3})+$)','ig'),"$1,$2     "))
1,000     000,000     000,000     000
```


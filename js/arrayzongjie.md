# Array总结

一个问题不能栽倒N次

## 参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)

所有的回调中，如果有`thisArg`，那么回调函数的`this`值就是`thisArg`。只对`function(){}`有效，对**箭头函数**不起作用。

## 返回新数组

### `Array.concat()`

浅拷贝

```javascript
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```

### `Array.from()`

```javascript
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]
```

### `Array.fill()`

语法：arr.fill\(value\[, start\[, end\]\]\)

### `Array.filter()`

创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。

### `Array.flat()`

替代方案：使用 reduce 与 concat

**深度递归遍历数组**

```javascript
var arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

var arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]

//使用 Infinity，可展开任意深度的嵌套数组
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

**移除空项**

```javascript
var arr4 = [1, 2, [3, 4, [5,, 6,, [7, ,8, [9,, 10]]]]];
arr4.flat(Infinity)
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### `Array.flatMap()`

与map\(\)相似，替代方案：使用 reduce 与 concat

```javascript
var arr1 = [1, 2, 3, 4];
console.log(arr1.flatMap(x => x * 2));     // [2, 4, 6, 8]
console.log(arr1.flatMap(x => [x * 2]));   // [2, 4, 6, 8]
console.log(arr1.flatMap(x => [[x * 2]])); // [[2], [4], [6], [8]]
```

### `Array.slice()`

返回一个新的数组对象，这一对象是一个由 `begin` 和 `end` 决定的原数组的浅拷贝

## 改变原数组

### `Array.copyWithin()`

浅复制数组的一部分到同一数组中的另一个位置

```javascript
[1, 2, 3, 4, 5].copyWithin(-2)
// [1, 2, 3, 1, 2]

[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2, -3, -1)
// [1, 2, 3, 3, 4]

[].copyWithin.call({length: 5, 3: 1}, 0, 3);
// {0: 1, 3: 1, length: 5}
```

### `Array.pop()`

删除最后一个元素，并返回该元素的值。

### `Array.push()`

将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

### `Array.reverse()`

### `Array.shift()`

方法从数组中删除第一个元素，并返回该元素的值。

### `Array.unshift()`

将一个或多个元素添加到数组的开头，并返回该数组的新长度。

### `Array.sort()`

9 出现在 80 之前，但因为（没有指明 compareFunction），比较的数字会先被转换为字符串，所以在Unicode顺序上 "80" 要比 "9" 要靠前。 **对非 ASCII 字符排序**

```javascript
var items = ['réservé', 'premier', 'cliché', 'communiqué', 'café', 'adieu'];
items.sort(function (a, b) {
  return a.localeCompare(b);
});

// items is ['adieu', 'café', 'cliché', 'communiqué', 'premier', 'réservé']
```

### `Array.splice()`

返回由被删除的元素组成的一个数组。

## 合成字符串

### `Array.join()`

### `Array.toLocaleString()`

```javascript
const array1 = [1, 'a', new Date('21 Dec 1997 14:12:00 UTC')];
const localeString = array1.toLocaleString('en', {timeZone: "UTC"});

console.log(localeString);
// expected output: "1,a,12/21/1997, 2:12:00 PM",

var prices = ['￥7', 500, 8123, 12];
prices.toLocaleString('ja-JP', { style: 'currency', currency: 'JPY' });

// "￥7,￥500,￥8,123,￥12"
```

### `Array.toString()`

返回一个用逗号相隔的字符串。

## 判断

### `Array.every()`

一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个**布尔值**。

### `Array.isArray()`

### `Array.includes()`

语法：arr.includes\(valueToFind\[, fromIndex\]\) 如果 fromIndex 大于等于数组的长度，则会返回 false，且该数组不会被搜索。

### `Array.some()`

如果用一个空数组进行测试，在任何情况下它返回的都是`false`。`callback`返回真值时结束。

## 查找

### `Array.find()`

返回数组中满足提供的测试函数的第一个元素的值。否则返回 `undefined`。

### `Array.findIndex()`

返回数组中满足提供的测试函数的第一个元素的**索引**。否则返回`-1`。

### `Array.indexOf()`

语法：arr.indexOf\(searchElement\[, fromIndex\]\) 如果该索引值大于或等于数组长度，意味着不会在数组里查找，返回-1。为负数也是**从前往后**查。

### `Array.lastIndexOf()`

语法：arr.lastIndexOf\(searchElement\[, fromIndex\]\) 如果该索引值大于或等于数组长度，意味着不会在数组里查找，返回-1。为负数也是**从后往前**查。

## 遍历

若你需要提前终止循环，你可以使用：

* 一个简单的 for 循环
* for...value...of / for...key...in 循环
* Array.every\(\)直到falsy
* Array.some\(\)直到truthy
* Array.find\(\)
* Array.findIndex\(\)
* Array.filter\(\) + [Array.forEach\(\)](arrayzongjie.md#arrayforeach) \(forEach本身只能通过抛出错误来中止\)

### `Array.keys()`

方法返回一个包含数组中每个**索引键**的Array Iterator对象。

### `Array.values()`

方法返回一个包含数组中每个**索引值**的Array Iterator对象。

### `Array.entries()`

返回一个新的 Array 迭代器\(Array Iterator\)对象， 对象上有`next()`方法，`next()`返回一个对象，对象的`value`对应的值是`["key","value"]`

**二维数组按行排序**

```javascript
function sortArr(arr) {
    var goNext = true;
    var entries = arr.entries();
    while (goNext) {
        var result = entries.next();
        if (result.done !== true) {
            result.value[1].sort((a, b) => a - b);
            goNext = true;
        } else {
            goNext = false;
        }
    }
    return arr;
}

var arr = [[1,34],[456,2,3,44,234],[4567,1,4,5,6],[34,78,23,1]];
sortArr(arr);
/*(4) [Array(2), Array(5), Array(5), Array(4)]
    0:(2) [1, 34]
    1:(5) [2, 3, 44, 234, 456]
    2:(5) [1, 4, 5, 6, 4567]
    3:(4) [1, 23, 34, 78]
    length:4
    __proto__:Array(0)
*/
```

**使用for…of 循环**

```javascript
var arr = ["a", "b", "c"];
var iterator = arr.entries();
// undefined

for (let e of iterator) {
    console.log(e);
}

// [0, "a"]
// [1, "b"]
// [2, "c"]
```

### `Array.forEach()`

方法对数组的每个元素执行一次给定的函数。**没有返回值**，已删除或者未初始化的项将被跳过，并且**不可链式调用**，**除了抛出异常以外，没有办法中止或跳出 forEach\(\) 循环**。

### `Array.map()`

返回一个新数组，其结果是该数组中的每个元素都调用一次提供的函数后的返回值。当你不打算使用返回的新数组却使用`map`是违背设计初衷的，请用`forEach`或者`for-of`替代。 map会跳过空项，如果没有返回值就是`undefined`

```javascript
var numbers = [1, 8, 4,7,10,, 9];
var doubles = numbers.map(function(num,index) {
    if(!(index%2)) {
        // 如果写!index%2，就会算成(!index)%2， 结果就是[2, undefined, undefined, undefined, undefined, empty, undefined]
        console.log(num, index%2)
        return num * 2
    };
});
console.log(doubles)
// 1 0
// 4 0
// 10 0
// 9 0
// [2, undefined, 8, undefined, 20, empty, 18]
```

### `Array.reduce()`

如果没有提供`initialValue`，`reduce` 会从索引1的地方开始执行 `callback` 方法，跳过第一个索引。如果提供`initialValue`，从索引0开始。 空数组必须有`initialValue`。 [**按顺序运行Promise**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

### `Array.reduceRight()`

与`reduce`是相反方向。

## 筛选查找总结

要查找的元素可能是基础类型`String`，`Number`，也可能是`Objet`，`Array`。 后者只能通过`callback`去判断它是不是**符合条件**

```javascript
var array = [['a', 'b'], 'a', 'c', 'a', 'd'];
var element = ['a', 'b'];
console.log(array.indexOf(element)) // -1
```

* 是否都：`every`返回布尔值
* 有没有：
  * `includes`，基础类型
  * `indexOf` 有就返回索引，没有-1
  * `some`，返回布尔值
  * `find` 有就返回值，没有`undefined`
  * `findIndex`，有就返回索引，没有-1
* 有哪些：`filter`返回符合条件的数组


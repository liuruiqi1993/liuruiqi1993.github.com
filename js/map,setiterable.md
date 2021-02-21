# Map,Set和iterable

## Map
`Map`是一组键值对的结构，具有极快的查找速度。初始化`Map`需要一个二维数组。
一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉。
```
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
```
**将Object转换为Map**
```
var obj = { foo: "bar", baz: 42 };
var map = new Map(Object.entries(obj));
console.log(map); // Map { foo: "bar", baz: 42 }
```

**将Map转换为Object**
```
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);
const obj = Object.fromEntries(entries);
console.log(obj);
// expected output: Object { foo: "bar", baz: 42 }
```
## Set
一组`key`的集合，但不存储`value`。由于`key`不能重复。
```
var s = new Set([1, 2, 3]);
s.add(4);
s.delete(3);
```
**数组去重**
```
Array.from(new Set(array))
```

## iterable
遍历`Array`可以采用下标循环，遍历`Map`和`Set`就无法使用下标。为了统一集合类型，ES6标准引入了新的`iterable`类型，`Array`、`Map`和`Set`都属于`iterable`类型。

具有`iterable`类型的集合可以通过新的`for ... of`和`forEach`循环来遍历。
```
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);
});

var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});

var a = ['A', 'B', 'C'];
a.forEach(function (element) {
    console.log(element);
});
```

# Object总结

对象的属性分为可枚举和不可枚举，他们是由property的enumerable值决定的。能被for...in到的就是可枚举。

```javascript
function Person() {
    this.name = 'a'
    this.age = 'b'
}
function Parent(){
    this.hobby = "c"
}
Parent.prototype = new Person()

let child = new Parent()
console.log(child)
for (let key in child){
    console.log(key)
}
// Parent {hobby: "c"}
// hobby
// name
// age

let ab = Object.create(child, {'property2': {
    value: 'Hello',
    writable: false
  }})
console.log(ab)
for (let key in ab){
    console.log(key)
}
// Person {property2: "Hello"}
// hobby
// name
// age
```

## 设定和获取设定

### ////////////////////

#### Object.seal\(\)

封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。**返回被密封的对象**。 使用`Object.freeze()`冻结的对象中的现有属性值是不可变的。用`Object.seal()`密封的对象可以改变其现有属性值。

```javascript
const object1 = {
  property1: 42
};

Object.seal(object1);
object1.property1 = 33;
console.log(object1.property1);
// expected output: 33
```

#### Object.isSealed\(\)

方法判断一个对象是否被密封。

#### Object.freeze\(\)

**冻结**一个对象后该对象的原型也不能被修改。**返回被冻结的对象**。 如果一个属性的值是个对象，则这个对象中的属性是可以修改的，除非它也是个冻结对象。

```javascript
const obj = {
  prop: 42
};

Object.freeze(obj);
obj.prop = 33;
// Throws an error in strict mode
console.log(obj.prop);
// expected output: 42
```

#### Object.isFrozen\(\)

判断一个对象是否被冻结。

#### Object.preventExtensions\(\)

让一个对象变的不可扩展，也就是永远不能再添加新的属性。**不可扩展的对象**。

#### Object.isExtensible\(\)

判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。`Object.preventExtensions`，`Object.seal` 或 `Object.freeze` 方法都可以标记一个对象为不可扩展。

### ////////////////////

#### Object.defineProperty\(\)

精确地添加或修改对象的属性。 语法：Object.defineProperty\(obj, prop, descriptor\) **返回obj。**

```javascript
function Archiver() {
  var temperature = null;
  var archive = [];

  Object.defineProperty(this, 'temperature', {
    get: function() {
      console.log('get!');
      return temperature;
    },
    set: function(value) {
      temperature = value;
      archive.push({ val: temperature });
    }
  });

  this.getArchive = function() { return archive; };
}

var arc = new Archiver();
arc.temperature; // 'get!'
arc.temperature = 11;
arc.temperature = 13;
arc.getArchive(); // [{ val: 11 }, { val: 13 }]
```

#### Object.getOwnPropertyDescriptor\(\)

返回指定对象上一个**自有属性**对应的属性描述符\(**对象**\)。不需要从原型链上进行查找的属性。

#### Object.defineProperties\(\)

一次定义多个 语法：Object.defineProperties\(obj, props\) **返回props**。

```javascript
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```

#### Object.getOwnPropertyDescriptors\(\)

一个对象的所有自身属性的描述符。

### ////////////////////

#### Object.hasOwnProperty\(\)

会返回一个**布尔值**，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。

#### Object.getOwnPropertyNames\(\)

返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但**不包括Symbol值**作为名称的属性）组成的**数组**。

```javascript
var arr = ["a", "b", "c"];
console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]
```

**只获取不可枚举的属性**

```javascript
var target = myObject;
var enum_and_nonenum = Object.getOwnPropertyNames(target);
var enum_only = Object.keys(target);
var nonenum_only = enum_and_nonenum.filter(function(key) {
    var indexInEnum = enum_only.indexOf(key);
    if (indexInEnum == -1) {
        // 没有发现在enum_only健集中意味着这个健是不可枚举的,
        // 因此返回true 以便让它保持在过滤结果中
        return true;
    } else {
        return false;
    }
});

console.log(nonenum_only);
```

#### Object.getOwnPropertySymbols\(\)

返回一个给定对象自身的所有 Symbol 属性的**数组**。

### ////////////////////

#### Object.getPrototypeOf\(\)

返回给定**对象的原型**。

```javascript
var proto = {};
var obj = Object.create(proto);
Object.getPrototypeOf(obj) === proto; // true

var reg = /a/;
Object.getPrototypeOf(reg) === RegExp.prototype; // true
```

#### Object.isPrototypeOf\(\)

调用对象是否在另一个对象的原型链上。返回**布尔值**。

#### Object.setPrototypeOf\(\)

设置一个指定的对象的原型。 更改对象的 \[\[Prototype\]\]在各个浏览器和 JavaScript 引擎上都是一个很慢的操作。 你应该使用 Object.create\(\)来创建带有你想要的\[\[Prototype\]\]的新对象。

### ////////////////////

#### Object.propertyIsEnumerable\(\)

返回一个布尔值，表示指定的属性是否可枚举。

## 新对象

### Object.assign\(\)

将所有可枚举属性的值从一个或多个源对象复制到目标对象。[**浅拷贝**](https://github.com/liuruiqi1993/liuruiqi1993.github.com/tree/5313af8d52ed2e186aa445ad360f3d7a5228b632/js/copy/深浅拷贝.html)

### Object.create\(\)

创建一个新对象，使用现有的对象来提供新创建的对象的**proto**。[**浅拷贝**](https://github.com/liuruiqi1993/liuruiqi1993.github.com/tree/5313af8d52ed2e186aa445ad360f3d7a5228b632/js/copy/深浅拷贝.html)

## 判断

### Object.is\(\)

判断两个值是否相同。如果下列任何一项成立，则两个值相同：

* 两个值都是 undefined
* 两个值都是 null
* 两个值都是 true 或者都是 false
* 两个值是由相同个数的字符按照相同的顺序组成的字符串
* 两个值指向同一个对象
* 两个值都是数字并且
  * 都是正零 +0
  * 都是负零 -0
  * 都是 NaN
  * 都是除零和 NaN 外的其它同一个数字

这种相等性判断逻辑和传统的 `==` 运算不同，`==` 运算符会对它两边的操作数做隐式类型转换（如果它们类型不同），然后才进行相等性比较，（所以才会有类似 `"" == false` 等于 `true` 的现象），但 **`Object.is` 不会做这种类型转换**。

```javascript
Object.is(window, window);   // true
Object.is([], []);           // false
Object.is(0, -0);            // false
Object.is(0, +0);            // true
Object.is(NaN, 0/0);         // true
```

## 遍历

### for...key...in

可枚举

### Object.keys\(\)

返回一个由一个给定对象的自身可枚举**属性**组成的数组，数组中属性名的排列顺序和使用 `for...in` 循环遍历该对象时返回的顺序一致。

### Object.values\(\)

返回一个给定对象自身的所有可枚举**属性值**的数组，值的顺序与使用for...in循环的顺序相同。

### Object.entries\(\)

返回一个给定对象自身可枚举属性的**键值对**数组，其排列与使用 `for...in` 循环遍历该对象时返回的顺序一致（区别在于 `for-in` 循环还会枚举原型链中的属性）。

```javascript
// getFoo is property which isn't enumerable
const myObj = Object.create({}, { getFoo: { value() { return this.foo; } } });
myObj.foo = 'bar';
console.log(Object.entries(myObj)); // [ ['foo', 'bar'] ]

// non-object argument will be coerced to an object
console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]
```

**将Object转换为Map**

```javascript
var obj = { foo: "bar", baz: 42 };
var map = new Map(Object.entries(obj));
console.log(map); // Map { foo: "bar", baz: 42 }
```

**将Map转换为Object**

```javascript
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);
const obj = Object.fromEntries(entries);
console.log(obj);
// expected output: Object { foo: "bar", baz: 42 }
```

## JSON

### JSON.stringify\(\)

```javascript
var xiaoming = {
    name: '小明',
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    'middle-school': '\"W3C\" Middle School',
    skills: ['JavaScript', 'Java', 'Python', 'Lisp']
};
// 1转换对象
// 2需要的字段，也可以是函数
// 3缩进
JSON.stringify(xiaoming, ['name', 'skills'], '  ');
```

想要精确控制如何序列化小明，可以给xiaoming定义一个`toJSON()`的方法，直接返回JSON应该序列化的数据：

```javascript
var xiaoming = {
    name: '小明',
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    'middle-school': '\"W3C\" Middle School',
    skills: ['JavaScript', 'Java', 'Python', 'Lisp'],
    toJSON () {
        return { // 只输出name和age，并且改变了key：
            'Name': this.name,
            'Age': this.age
        };
    }
};

JSON.stringify(xiaoming); // '{"Name":"小明","Age":14}'
```

### JSON.parse\(\)

JSON.parse\(\)还可以接收一个函数，用来转换解析出的属性：

```javascript
var obj = JSON.parse('{"name":"小明","age":14}', (key, value) => {
    if (key === 'name') {
        return value + '同学';
    }
    return value;
});
console.log(JSON.stringify(obj)); // {name: '小明同学', age: 14}
```


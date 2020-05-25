---
layout: post
title: TypeScript笔记
---
* 对值所具有的结构进行类型检查。
## 基础类型
```
// boolean
let isDone: boolean = false;

// number
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;

//string
let name: string = "bob";
let sentence: string = `Hello, my name is ${ name }.

//array
let list: number[] = [1, 2, 3];
let list: any[] = [1, true, "free"];
let list: Array<number> = [1, 2, 3];

//元组 Tuple 按顺序
let x: [string, number];
x = ['hello', 10]; // OK
x = [10, 'hello']; // Error

//枚举
//使用枚举类型可以为一组数值赋予友好的名字，默认情况下，从0开始为元素编号。
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];
console.log(colorName);  // 显示'Green'因为上面代码里它的值是2

//Any
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean

//Void，Null 和 Undefined
//没有任何类型，当一个函数没有返回值时，你通常会见到其返回值类型是 void
//null和undefined是所有类型的子类型，可以把 null和undefined赋值给number类型的变量
function warnUser(): void {
    console.log("This is my warning message");
}

let unusable: void = undefined;
let u: undefined = undefined;
let n: null = null;

//Never
//`never`类型表示的是那些永不存在的值的类型。`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使 `any`也不可以赋值给`never`。

// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}

//Object
表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。
```

## 变量声明
### 解构函数
```
function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
f(input);

let {a, b}: {a: string, b: number} = o;

function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}
```
## 接口
为类型命名和为你代码定义契约。

类型检查器会查看`printLabel`的调用。 `printLabel`有一个参数，并要求这个对象参数有一个名为`label`类型为`string`的属性。 
```
function printLabel(labelledObj: { label: string }) {
  console.log(labelledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```
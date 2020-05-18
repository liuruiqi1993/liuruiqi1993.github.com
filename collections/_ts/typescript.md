---
layout: post
title: TypeScript笔记
---
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

```
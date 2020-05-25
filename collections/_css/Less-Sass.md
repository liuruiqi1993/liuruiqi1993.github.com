---
layout: post
title: less and sass
tags: 
---
## Less
### 变量
```
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```
任何 ~"anything" 或 ~'anything' 形式的内容都将按原样输出，除非 interpolation。
```
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
// or 从 Less 3.5 开始，可以简写为：
@min768: (min-width: 768px);
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```
URLs
```
// Variables
@images: "../img";

// Usage
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
```
变量嵌套
```
@primary:  green;
@secondary: blue;

.section {
  @color: primary;

  .element {
    color: @@color;
  }
}
```
使用已有属性的值
```
.widget {
  color: #efefef;
  background-color: $color;
}
```



### mixin

```
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu a {
  color: #111;
  .bordered();
}
```
可以通过中括号查看mixin里的属性`mixin[prop];`，如果是空括号[]就会查询最后一条。

mixin中的变量能被调用者访问到, 作用域范围在调用者的大括号中。
```
.mixin() {
  @width:  100%;
  @height: 200px;
}

.caller {
  .mixin();
  width:  @width;
  height: @height;
}
```
转译成
```
.caller {
  width:  100%;
  height: 200px;
}
```
带参数
```
.border-radius(@radius) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}
```
默认值
```
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}
```
#### @arguments
```
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000) {
  -webkit-box-shadow: @arguments;
     -moz-box-shadow: @arguments;
          box-shadow: @arguments;
}
.big-block {
  .box-shadow(2px; 5px);
}
```


####  @rest
```
.mixin(...) {        // matches 0-N arguments
.mixin() {           // matches exactly 0 arguments
.mixin(@a: 1) {      // matches 0-1 arguments
.mixin(@a: 1; ...) { // matches 0-N arguments
.mixin(@a; ...) {    // matches 1-N arguments
.mixin(@a; @rest...) {
   // @rest is bound to arguments after @a
   // @arguments is bound to all arguments
}

```

#### 集合
```
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}

#header a {
  color: orange;
  #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```
#### 将Mixin作为函数
```
.average(@x, @y) {
  @result: ((@x + @y) / 2);
}

div {
  // 调用@result并查询@result的值
  padding: .average(16px, 50px)[@result];
}
```
#### 循环loop
```
.generate-columns(4);

.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));
}
```
转译为
```
.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}
```

#### 匹配
```
.mixin(dark; @color) {
  color: darken(@color, 10%);
}
.mixin(light; @color) {
  color: lighten(@color, 10%);
}
.mixin(@_; @color) {
  display: block;
}

@switch: light;

.class {
  .mixin(@switch; #888);
}
```
因为`.mixin(@switch; #888)`既符合`.mixin(light; @color)`又符合`.mixin(@_; @color)`，所以转译成
```
.class {
  color: #a2a2a2;
  display: block;
}
```
### 映射Map
```
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```
### @import
```
// Variables
@themes: "../../src/themes";

// Usage
@import "@{themes}/tidal-wave.less";
```
```
@import "library"; // library.less
@import "typo.css";
```
### &
如果插在&前面
```
.header {
  .menu {
    border-radius: 5px;
    .no-borderradius & {
      background-image: url('images/button-background.png');
    }
  }
}
```
转译成
```
.header .menu {
  border-radius: 5px;
}
.no-borderradius .header .menu {
  background-image: url('images/button-background.png');
}
```
```
p, a, ul, li {
  border-top: 2px dotted #366;
  & + & {
    border-top: 0;
  }
}
```
转成
```
p,
a,
ul,
li {
  border-top: 2px dotted #366;
}
p + p,
p + a,
p + ul,
p + li,
a + p,
a + a,
a + ul,
a + li,
ul + p,
ul + a,
ul + ul,
ul + li,
li + p,
li + a,
li + ul,
li + li {
  border-top: 0;
}
```

### extend
```
nav ul {
  &:extend(.inline);
  background: blue;
}
.inline {
  color: red;
}
```
```
.a:extend(.b) {}

// 等价于
.a {
  &:extend(.b);
}
```
```
.c:extend(.d all) {
  // 继承所有的".d" 包括".x.d"和".d.x"
}
.c:extend(.d) {
  // 只继承".d"
}
```
```
.e:extend(.f) {}
.e:extend(.g) {}

// 等价于
.e:extend(.f, .g) {}
```

### 合并
用+ or +_ 标志
```
.mixin() {
  box-shadow+: inset 0 0 10px #555;
}
.myclass {
  .mixin();
  box-shadow+: 0 0 20px black;
}
```
转译成带逗号
```
.myclass {
  box-shadow: inset 0 0 10px #555, 0 0 20px black;
}
```
```
.mixin() {
  transform+_: scale(2);
}
.myclass {
  .mixin();
  transform+_: rotate(15deg);
}
```
转译成带空格
```
.myclass {
  transform: scale(2) rotate(15deg);
}
```
### [less函数](https://less.bootcss.com/functions/)


## Sass
### scss与sass的区别
SCSS 语法使用 .scss 文件扩展名。除了极少部分的例外， 它是 CSS 的超集，也就是说 所有有效的 CSS 也同样都是有效的 SCSS 。
CSS 规定了如何从大多数错误中恢复， 而不是立即失败。

缩进语法是 Sass 的原始语法，因此它使用文件 扩展名 .sass 。由于这个扩展名的原因，这种语法有时直接被称为 “Sass"。
Sass 样式表是经由 Unicode 编码序列解析而来的。 解析是直接进行的，没有转换为标记流（token stream）的过程。
当 Sass 在样式表中遇到无效语法时，解析将失败， 并向用户展示错误信息。
```
//SCSS有花括号

// This comment won't be included in the CSS.

/* But this comment will, except in compressed mode. */

@mixin button-base() {
  @include typography(button);
  &:hover { cursor: pointer; }
  &:disabled {
    color: $mdc-button-disabled-ink-color;
    cursor: default;
    pointer-events: none;
  }
}

//SASS用缩进

// This comment won't be included in the CSS.
   This is also commented out.

/* But this comment will, except in compressed mode.

@mixin button-base()
  @include typography(button)
  &:hover
    cursor: pointer
  &:disabled
    color: $mdc-button-disabled-ink-color
    cursor: default
    pointer-events: none
```
### 嵌套
属性也是可以嵌套的
```
p {
　　border: {
　　　　color: red;
　　}
}
```
### 变量
```
$myFont: Helvetica, sans-serif; // 全局作用域
body {
  $myFontSize: 18px; // 局部作用域
  $myColor: green !global;  // 全局作用域
  font-family: $myFont;
  font-size: $myFontSize;
}
p {
  color: $myColor;
}
```
### 映射Map
```
@use "sass:map";

$theme-colors: (
  "success": #28a745,
  "info": #17a2b8,
  "warning": #ffc107,
);

.alert {
  // Instead of $theme-color-#{warning}
  background-color: map.get($theme-colors, "warning");
}
```
### @use
`@use <url> with (<variable>: <value>, <variable>: <value>)`
```
// _library.sass
$black: #000 !default
$border-radius: 0.25rem !default
$box-shadow: 0 0.5rem 1rem rgba($black, 0.15) !default

code
  border-radius: $border-radius
  box-shadow: $box-shadow

// style.sass
@use 'library' with ($black: #222, $border-radius: 0.1rem)
```
转译成
```
code {
  border-radius: 0.1rem;
  box-shadow: 0 0.5rem 1rem rgba(34, 34, 34, 0.15);
}
```
```
@use "sass:math" as math

// This assignment will fail.
math.$pi: 0
```
### [&](https://sass.bootcss.com/documentation/style-rules/parent-selector)
```
.alert {
  &:hover {
    font-weight: bold;
  }
  [dir=rtl] & {
    margin-left: 0;
    margin-right: 10px;
  }
  :not(&) {
    opacity: 0.8;
  }
}
```
转换成
```
.alert:hover {
  font-weight: bold;
}
[dir=rtl] .alert {
  margin-left: 0;
  margin-right: 10px;
}
:not(.alert) {
  opacity: 0.8;
}
```
### placeholder %
%toolbelt不会被编译成选择器，只能和@extended一起用。
```
%toolbelt {
  box-sizing: border-box;
  border-top: 1px rgba(#000, .12) solid;
  padding: 16px 0;
  width: 100%;

  &:hover { border: 2px rgba(#000, .5) solid; }
}

.action-buttons {
  @extend %toolbelt;
  color: #4285f4;
}

.reset-buttons {
  @extend %toolbelt;
  color: #cddc39;
}
```

### @import
在文件名的开头添加一个下划线，Sass 的代码文件就不会编译到一个 CSS 文件。
带下划线与不带下划线的同名文件放置在同一个目录下，比如，_colors.scss 和 colors.scss 不能同时存在于同一个目录下，
否则带下划线的文件将会被忽略。
```
@import "variables";
@import "colors";
@import "reset";
```

### @mixin 与 @include
@mixin 指令允许我们定义一个可以在整个样式表中重复使用的样式。

@include 指令可以将混入（mixin）引入到文档中。

```
// 连接符号 - 与下划线符号 _ 是相同的，也就是 @mixin important-text { } 与 @mixin important_text { } 是一样的混入。
@mixin important-text {
  color: red;
  font-size: 25px;
  font-weight: bold;
  border: 1px solid blue;
}

.danger {
  @include important-text;
  background-color: green;
}

```
```
//混入中也可以包含混入
@mixin special-text {
  @include important-text;
  @include link;
  @include special-border;
}

```
```
// 混入传参
@mixin bordered($color, $width) {
  border: $width solid $color;
}

.myArticle {
  @include bordered(blue, 1px);  // 调用混入，并传递两个参数
}

```
```
// 混入传参默认值
@mixin sexy-border($color, $width: 1in) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}

```
```
// 不确定参数个数，用...
@mixin box-shadow($shadows...) {
      -moz-box-shadow: $shadows;
      -webkit-box-shadow: $shadows;
      box-shadow: $shadows;
}

.shadows {
  @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
}

```
```
//适配浏览器前缀
@mixin transform($property) {
  -webkit-transform: $property;
  -ms-transform: $property;
  transform: $property;
}

.myBox {
  @include transform(rotate(20deg));
}
```

### @extend 与 继承
如果一个样式与另外一个样式几乎相同，只有少量的区别，则使用 @extend，告诉 Sass 一个选择器的样式从另一选择器继承。
```
.button-basic  {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

.button-report  {
  @extend .button-basic;
  background-color: red;
}

.button-submit  {
  @extend .button-basic;
  background-color: green;
  color: white;
}
```
### 控制流
### @for in
```
@for $i from 1 to 10 {
　　　　.border-#{$i} {
　　　　　　border: #{$i}px solid blue;
　　　　}
　　}
```
#### @while
```
$i: 6;

　　@while $i > 0 {
　　　　.item-#{$i} { width: 2em * $i; }
　　　　$i: $i - 2;
　　}
```
#### @each
```
@mixin prefix($property, $value, $prefixes)
  @each $prefix in $prefixes
    -#{$prefix}-#{$property}: $value

  #{$property}: $value

.gray
  @include prefix(filter, grayscale(50%), moz webkit)
```
#### @if
```
$rounded-corners: false;
.button {
  border: 1px solid black;
  border-radius: if($rounded-corners, 5px, null);
}

```
```
$dark-theme: true !default
@if $dark-theme
  $primary-color: darken($primary-color, 60%)
  $accent-color: lighten($accent-color, 60%)
```

### sass函数
SASS封好的一些方法。
[https://www.runoob.com/sass/sass-functions.html](https://www.runoob.com/sass/sass-functions.html)

### 自定义属性
```
$primary: #81899b;
$accent: #302e24;
$warn: #dfa612;

:root {
  --primary: #{$primary};
  --accent: #{$accent};
  --warn: #{$warn};

  // Even though this looks like a Sass variable, it's valid CSS so it's not evaluated.
  --consumed-by-js: $primary;
}
```
### 自定义函数
```
@function double($n) {
　　@return $n * 2;
}

#sidebar {
　　width: double(5px);
}
```

# css选择器

看到了这一篇[https://juejin.im/post/5e6b2b9ce51d4526fd069b87](https://juejin.im/post/5e6b2b9ce51d4526fd069b87) 所以CSS选择器查缺补漏，安排上

| 基础 |  |
| :--- | :--- |
| element1~element2 | 选择element1之后的每一个element2 |
| a\[src^="https"\] | 选择每一个src属性的值以"https"开头的元素 |
| a\[src$=".pdf"\] | 选择每一个src属性的值以".pdf"结尾的元素 |
| a\[src\*="runoob"\] | 选择每一个src属性的值包含子字符串"runoob"的元素 |
| p:first-of-type | 选择每个p元素是其父级的第一个p元素 |
| p:last-of-type | 选择每个p元素是其父级的最后一个p元素 |
| p:only-of-type | 选择每个p元素是其父级的唯一p元素 |
| p:nth-of-type\(2\) | 选择每个p元素是其父级的第二个p元素 |
| :not\(p\) | 选择每个并非p元素的元素 |
| **特殊** |  |
| p:only-child | 选择每个p元素是其父级的唯一子元素 |
| p:empty | 选择每个没有任何子级的p元素（包括文本节点） |
| :target | 选择当前锚点 |
| :root | 选择文档的根元素,在HTML中根元素始终是HTML元素。 |
| ::selection | 匹配元素中被用户选中或处于高亮状态的部分 |
| **表单** |  |
| input:enabled | 选择每一个已启用的输入元素，与input:disabled相反 |
| input:optional | 用于匹配可选的输入元素 和:required相反 |
| :valid | 用于匹配输入值为合法的元素和:invalid相反 |

## 组合

### 1. :active伪类与CSS数据上报

```css
.button-1:active::after {
    content: url(./pixel.gif?action=click&id=button1);
    display: none;
}
.button-2:active::after {
    content: url(./pixel.gif?action=click&id=button2);
    display: none;
}
```

### 2. 超实用超高频使用的:empty 伪类

#### 2.1 隐藏空元素

```css
.cs-module:empty {
    display: none;
}
```

#### 2.2 字段缺失智能提示

```css
td:empty::before{
    content: '-';
    color: gray;
}

公共类
.cs-search-module:empty::before{
    content: '没有搜索结果'；
    display: block;
    line-height: 300px;
    text-align: center;
    color: gray;
}
```

## 3. 用好:only-child伪类

![&apos;&apos;](https://user-gold-cdn.xitu.io/2020/3/13/170d2a24152f116a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```markup
<!-- 1. 只有加载图片 -->
<div class="cs-loading">
    <img src="./loading.png" class="cs-loading-img">
</div>
<!-- 2. 只有加载文字 -->
<div class="cs-loading">
    <p class="cs-loading-p">正在加载中...</p>
</div>
<!-- 3. 加载图片和加载文字同时存在 -->
<div class="cs-loading">
    <img src="./loading.png" class="cs-loading-img">
    <p class="cs-loading-p">正在加载中...</p>
</div>
```

```css
.cs-loading {
    height: 150px;
    position: relative;
    text-align: center;
    /* 与截图无关，截图示意用 */
    border: 1px dotted;
}
/* 图片和文字同时存在时在中间留点间距 */
.cs-loading-img {
    width: 32px; height: 32px;
    margin-top: 45px;
    vertical-align: bottom;
}
.cs-loading-p {
    margin: .5em 0 0;
    color: gray;
}
/* 只有图片的时候居中绝对定位 */
.cs-loading-img:only-child {
    position: absolute;
    left: 0; right: 0; top: 0; bottom: 0;
    margin: auto;
}
/* 只有文字的时候行号近似垂直居中 */
.cs-loading-p:only-child {
    margin: 0;
    line-height: 150px;
}
```


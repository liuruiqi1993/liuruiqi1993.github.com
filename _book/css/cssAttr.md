# CSS属性

|**@** | |
|-- | --|
|@charset | 定义样式表使用的字符集，比如content中使用。|
|@import | 告诉 CSS 引擎引入一个外部样式表。@import url list-of-media-queries;|
|@namespace | 告诉 CSS 引擎必须考虑XML命名空间。|
|@media | 如果满足媒介查询的条件则条件规则组里的规则生效。|
|@page | 描述打印文档时布局的变化。|
|@font-face | 描述将下载的外部的字体。|
|@keyframes | 描述 CSS 动画的中间步骤。|
|@supports | 特性查询，如果满足给定条件则条件规则组里的规则生效，ie不兼容。|
|@document | 如果文档样式表满足给定条件则条件规则组里的规则生效。|
|**function** | |
|expression() | ie5+|
|calc() | |
|min() | `min(40%, 400px)`兼容不及calc()|
|max() | `max(40%, 400px)`兼容不及calc()|
|clamp()| `clamp(1rem, 2.5vw, 1.5rem)`兼容不及calc()|
|attr()|`attr(data-width px, inherit);` 兼容性|
|**背景** ||
|background | bg-color <br>bg-image <br>position/bg-size <br>bg-repeat <br>bg-origin (相对位置padding-box border-box content-box) <br>bg-clip (绘制范围padding-box border-box content-box) <br>bg-attachment (背景图像是否固定或者随着页面的其余部分滚动) <br>background-position用top 或0px时都是以背景和盒子的左上角的原点。用百分比时，是图片本身（x%,y%）的那个点，与背景区域的（x%,y%）的那个点重合
|**border** | source <br>slice(向内偏移截取) <br>width <br>outset <br>repeat|
|border-style | dotted <br>dashed <br>solid <br>double <br>groove(外凹中凸) <br>ridge(外凸中凹) <br>inset(凹槽) <br>outset(凸起)|
|border-image | source <br>slice(留下边角，中间掏空) <br>width <br>outset(边框外移程度) <br>repeat|
|***三角形***| div {<br>width: 0;<br>height: 0;<br>border: 40px solid;<br>border-color: transparent transparent red;<br>}|
|**outline** | 1. 和border顺序相反(先颜色最后宽度)<br>2. 在border之外<br>3. outline-offset偏移距离(兼容性)|
|**内容生成**||
|counter-increment |在元素前面添加编号<br>counter-increment:section; <br>content:"Section" counter(section) ". ";|
|counter-reset | 最顶级是body编号为0，所以去掉0，就给body加上reset|
|quotes | 设置嵌套引用的引号类型|
|**list**||
|list-style | list-style-type <br>list-style-position (inside, outside ) <br>list-style-image.|
|**多列(Multi-column) 属性**||
|column-count | 列数|
|column-gap | |
|column-rule | column-rule-width <br>column-rule-style <br>column-rule-color|
|columns | column-width(默认auto) column-count;|
|**定位（Positioning） 属性** | |
|position | relative(相对于其正常位置进行定位)|
|clip | 搭配fixed和absolute用(和padding的区别是，padding-box里还看得到，但clip区域外是透明的)
|float && clear | 浮动 && 清除浮动|
|**分页（Print） 属性** | 打印时才生效|
|page-break-before &&  <br>page-break-after && <br>page-break-inside(元素中) | 分页符|
|**表格（Table） 属性** | |
|border-collapse | collapse(合并重复边框) <br> separate|
|border-spacing | cell间距|
|empty-cells | 空单元格(hide, show)|
|table-layout | 宽度fixed automatic|
|[**WebKit Extensions**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/WebKit_Extensions) ||
|-webkit-line-clamp | 把块容器中的内容限制为指定的行数。溢出缩略...|
|-webkit-tap-highlight-color | 设置点击链接的时候出现的高亮颜色|

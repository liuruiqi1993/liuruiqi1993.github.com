# Window

## 方法

|  |  |
| :--- | :--- |
| open\(\) | 打开一个新的浏览器窗口或查找一个已命名的窗口。 |
| close\(\) | 关闭浏览器窗口。通过 JavaScript 代码打开的窗口才能够由 JavaScript 代码关闭。 |
| print\(\) | 打印当前窗口的内容。 |
| confirm\(\) | 显示带有一段消息以及确认按钮和取消按钮的对话框。`confirm("提示!")`是**布尔值** |
| prompt\(\) | 显示可提示用户输入的对话框。`prompt("提示","placeholder")`是**String** |

## 事件

### onload

`DOMContentLoaded` 与`onload`区别以及使用

1. 当 `onload` 事件触发时，页面上所有的DOM，样式表，脚本，图片，flash都已经加载完成了。
2. 当 `DOMContentLoaded` 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash。

## navigator

**浏览器信息**

* `navigator.appName`：浏览器名称；
* `navigator.appVersion`：浏览器版本；
* `navigator.language`：浏览器设置的语言；
* `navigator.platform`：操作系统类型；
* `navigator.userAgent`：浏览器设定的User-Agent字符串。

## location

![](../../.gitbook/assets/20190505114702374.png)



|  |  |
| :--- | :--- |
| assign\(\) | 加载新的文档。 |
| reload\(\) | 重新加载当前文档。 |
| replace\(\) | 用新的文档替换当前文档。 |

## screen

|  |  |
| :--- | :--- |
| availHeight | 返回显示屏幕的高度 \(除 Windows 任务栏之外\)。 |
| availWidth | 返回显示屏幕的宽度 \(除 Windows 任务栏之外\)。 |
| bufferDepth | 设置或返回调色板的比特深度。 |
| colorDepth | 返回目标设备或缓冲器上的调色板的比特深度。 |
| deviceXDPI | 返回显示屏幕的每英寸水平点数。 |
| deviceYDPI | 返回显示屏幕的每英寸垂直点数。 |
| fontSmoothingEnabled | 返回用户是否在显示控制面板中启用了字体平滑。 |
| height | 返回显示屏幕的高度。 |
| logicalXDPI | 返回显示屏幕每英寸的水平方向的常规点数。 |
| logicalYDPI | 返回显示屏幕每英寸的垂直方向的常规点数。 |
| pixelDepth | 返回显示屏幕的颜色分辨率（比特每像素）。 |
| updateInterval | 设置或返回屏幕的刷新率。 |
| width | 返回显示器屏幕的宽度。 |

## document

### document.write, document.open, document.close

`document.open`, `document.close`的作用就是打开输出流。 文档在加载的过程中实际是一边加载一边用 `document.write` 写出内容到屏幕上，而**加载完成后，输出流关闭**，再调用 `document.write` 往网页上写内容的话，就会重写 `document`。

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>

 <p>JavaScript 能够直接写入 HTML 输出流中：</p>
 <script>
document.write("<h1>This is a heading</h1>");
document.write("<p>This is a paragraph.</p>");
 </script>

 <p>
您只能在 HTML 输出流中使用 <strong>document.write</strong>。
如果您在文档已加载后使用它（比如在函数中），会覆盖整个文档。
</p>

<button onclick="myFunction()">点击这里</button>

<script>
function myFunction()
{
    document.write("调用了函数，之前的文档被重写");
}
</script>

</body>
 </html>
```

没有`close`的话，标签页头一直转，加载中

```javascript
var w = window.open();
w.document.open();
w.document.write("<h1>Hello World!</h1>");
w.document.write("<h2>Hello World!</h2>");
w.document.close();
```

### document.readyState

`readyState` 属性返回当前文档的状态，返回以下值:

* uninitialized - 还未开始载入
* loading - 载入中
* interactive - 已加载，文档与用户可以开始交互
* complete - 载入完成

### document.scripts

返回文档中所有 `<script>` 元素的集合。

## history

|  |  |
| :--- | :--- |
| back\(\) | 加载 history 列表中的前一个 URL。 |
| forward\(\) | 加载 history 列表中的下一个 URL。 |
| go\(\) | 加载 history 列表中的某个具体页面。 |


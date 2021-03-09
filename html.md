# HTML



### 增

* document.createElement
* element.appendChild **语法node.appendChild\(node\)**
* element.insertBefore **语法node.insertBefore\(newnode,existingnode\)** 在`node`中的`existingnode`前插入`newnode`

如果你想创建一个新的文本列表项，

1. 首先创建一个节点，
2. 然后创建一个文本节点，
3. 然后将文本节点添加到LI节点上。
4. 最后将节点添加到列表中。

```javascript
var node=document.createElement("LI");
var textnode=document.createTextNode("Water");
node.appendChild(textnode);
document.getElementById("myList").appendChild(node);
```

### 删

**首先要获得该节点本身以及它的父节点，然后，调用父节点的`removeChild`把自己删掉** 当你遍历一个父节点的子节点并进行删除操作时，要注意，`children`属性是一个只读属性，并且它在子节点变化时会实时更新。

例如，对于如下HTML结构：

```javascript
<div id="parent">
    <p>First</p>
    <p>Second</p>
</div>
```

当我们用如下代码删除子节点时：

```javascript
var parent = document.getElementById('parent');
parent.removeChild(parent.children[0]);
parent.removeChild(parent.children[1]); // <-- 浏览器报错
```

浏览器报错：`parent.children[1]`不是一个有效的节点。原因就在于，当`<p>First</p>`节点被删除后，`parent.children`的节点数量已经从2变为了1，索引`[1]`已经不存在了。

因此，删除多个节点时，要注意`children`属性时刻都在变化。

### 改

* element.innerHTML

  不但可以修改一个DOM节点的文本内容，还可以直接通过HTML片段修改DOM节点内部的子树

  **如果写入的字符串是通过网络拿到了，要注意对字符编码来避免XSS攻击。**

* element.innerText

  不返回隐藏元素的文本

* textContent

  返回所有文本，&gt; IE9

* element.style

  驼峰式命名

### 查

* getElementById\(\)
* getElementsByClassName\(\)
* getElementsByTagName\(\)
* querySelector\(\)
* querySelectorAll\(\)


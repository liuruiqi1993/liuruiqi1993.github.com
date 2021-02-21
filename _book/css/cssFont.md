# CSS字体

## 属性

|**字体** ||
|--|--||
|font | "font-style font-variant font-weight font-size/line-height font-family"|
|font-style | normal italic oblique inherit|
|font-variant | normal small-caps inherit|
|font-weight ||
|font-size/line-height ||
|font-family ||
|**段落** ||
|direction && writing-mode | writing-mode样式多， IE兼容性不好|
|letter-spacing && word-spacing| 字符间距 单词距离|
|text-transform | capitalize uppercase lowercase|
|vertical-align | ![vertical-align](image/20200414191706.jpg)|
|white-space | pre(原格式，超出不断行) <br>nowrap(直到遇到break) <br>pre-wrap(保留空格断行) <br>pre-line(合并空格，保留换行)|
|text-align-last | 文本最后一行|
|word-break | normal(浏览器) <br>break-all(随意) <br>keep-all(半角空格或连字符处)|
|word-wrap | break-word(在长单词或 URL 地址内部进行换行)|

## 自定义字体 @font-face

```css
@font-face {
    font-family: myFirstFont;
    src: url('Sansation_Light.ttf'),
         url('Sansation_Light.eot'); /* IE9 */
}
```

## 安全字体

"安全字体"，因为它可以保证所有用户的视觉效果是一样的。
<table class="reference">
<tbody><tr>
<th width="55%" align="left">Serif 字体</th>
<th align="left">文本示例</th>
</tr>
<tr>
<td>Georgia, serif</td>
<td><p style="margin-top:0px;font-family: Georgia, serif">这是标题</p><p style="margin-bottom:0px;font-family: Georgia, serif">This is a paragraph</p></td>
</tr>
<tr>
<td>"Palatino Linotype", "Book Antiqua", Palatino, serif</td>
<td><p style="margin-top:0px;font-family: 'Palatino Linotype', 'Book Antiqua', Palatino, serif">这是标题</p><p style="margin-bottom:0px;font-family: 'Palatino Linotype', 'Book Antiqua', Palatino, serif">This is a paragraph</p></td>
</tr>
<tr>
<td>"Times New Roman", Times, serif</td>
<td><p style="margin-top:0px;font-family: 'Times New Roman', Times, serif">这是标题</p><p style="margin-bottom:0px;font-family: 'Times New Roman', Times, serif">This is a paragraph</p></td>
</tr>
<tr>
<th width="55%" align="left">sans - serif字体</th>
<th align="left">文本示例</th>
</tr>
<tr>
<td>Arial, Helvetica, sans-serif</td>
<td><p style="margin-top:0px;font-family: Arial, Helvetica, sans-serif">这是标题</p><p style="margin-bottom:0px;font-family: Arial, Helvetica, sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<td>Arial Black, Gadget, sans-serif</td>
<td><p style="margin-top:0px;font-family: Arial Black, Gadget, sans-serif;font-weight:normal">这是标题</p><p style="margin-bottom:0px;font-family: Arial Black, Gadget, sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<td>"Comic Sans MS", cursive, sans-serif</td>
<td><p style="margin-top:0px;font-family: 'Comic Sans MS', cursive, sans-serif">这是标题</p><p style="margin-bottom:0px;font-family: 'Comic Sans MS', cursive, sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<td>Impact, Charcoal, sans-serif</td>
<td><p style="margin-top:0px;font-family: Impact, Charcoal, sans-serif;font-weight:normal">这是标题</p><p style="margin-bottom:0px;font-family: Impact, Charcoal, sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<td>"Lucida Sans Unicode", "Lucida Grande", sans-serif</td>
<td><p style="margin-top:0px;font-family: 'Lucida Sans Unicode', 'Lucida Grande', sans-serif">这是标题</p><p style="margin-bottom:0px;font-family: 'Lucida Sans Unicode', 'Lucida Grande', sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<td>Tahoma, Geneva, sans-serif</td>
<td><p style="margin-top:0px;font-family: Tahoma, Geneva, sans-serif">这是标题</p><p style="margin-bottom:0px;font-family: Tahoma, Geneva, sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<td>"Trebuchet MS", Helvetica, sans-serif</td>
<td><p style="margin-top:0px;font-family: 'Trebuchet MS', Helvetica, sans-serif">这是标题</p><p style="margin-bottom:0px;font-family: 'Trebuchet MS', Helvetica, sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<td>Verdana, Geneva, sans-serif</td>
<td><p style="margin-top:0px;font-family: Verdana, Geneva, sans-serif">这是标题</p><p style="margin-bottom:0px;font-family: Verdana, Geneva, sans-serif">This is a paragraph</p></td>
</tr>
<tr>
<th width="55%" align="left">Monospace 字体</th>
<th align="left">文本示例</th>
</tr>
<tr>
<td>"Courier New", Courier, monospace</td>
<td><p style="margin-top:0px;font-family: 'Courier New', Courier, monospace">这是标题</p><p style="margin-bottom:0px;font-family: 'Courier New', Courier, monospace">This is a paragraph</p></td>
</tr>
<tr>
<td>"Lucida Console", Monaco, monospace</td>
<td><p style="margin-top:0px;font-family: 'Lucida Console', Monaco, monospace">这是标题</p><p style="margin-bottom:0px;font-family: 'Lucida Console', Monaco, monospace">This is a paragraph</p></td>
</tr>
</tbody></table>

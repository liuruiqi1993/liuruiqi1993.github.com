# css字体

## 属性

<table>
  <thead>
    <tr>
      <th style="text-align:left">&lt;b&gt;&lt;/b&gt;</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>&#x5B57;&#x4F53;</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">font</td>
      <td style="text-align:left">&quot;font-style font-variant font-weight font-size/line-height font-family&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left">font-style</td>
      <td style="text-align:left">normal italic oblique inherit</td>
    </tr>
    <tr>
      <td style="text-align:left">font-variant</td>
      <td style="text-align:left">normal small-caps inherit</td>
    </tr>
    <tr>
      <td style="text-align:left">font-weight</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">font-size/line-height</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">font-family</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x6BB5;&#x843D;</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">direction &amp;&amp; writing-mode</td>
      <td style="text-align:left">writing-mode&#x6837;&#x5F0F;&#x591A;&#xFF0C; IE&#x517C;&#x5BB9;&#x6027;&#x4E0D;&#x597D;</td>
    </tr>
    <tr>
      <td style="text-align:left">letter-spacing &amp;&amp; word-spacing</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x95F4;&#x8DDD; &#x5355;&#x8BCD;&#x8DDD;&#x79BB;</td>
    </tr>
    <tr>
      <td style="text-align:left">text-transform</td>
      <td style="text-align:left">capitalize uppercase lowercase</td>
    </tr>
    <tr>
      <td style="text-align:left">vertical-align</td>
      <td style="text-align:left">
        <img src="../.gitbook/assets/20200414191706.jpg" alt="vertical-align"
        />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">white-space</td>
      <td style="text-align:left">
        <p>pre(&#x539F;&#x683C;&#x5F0F;&#xFF0C;&#x8D85;&#x51FA;&#x4E0D;&#x65AD;&#x884C;)</p>
        <p>nowrap(&#x76F4;&#x5230;&#x9047;&#x5230;break)</p>
        <p>pre-wrap(&#x4FDD;&#x7559;&#x7A7A;&#x683C;&#x65AD;&#x884C;)</p>
        <p>pre-line(&#x5408;&#x5E76;&#x7A7A;&#x683C;&#xFF0C;&#x4FDD;&#x7559;&#x6362;&#x884C;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">text-align-last</td>
      <td style="text-align:left">&#x6587;&#x672C;&#x6700;&#x540E;&#x4E00;&#x884C;</td>
    </tr>
    <tr>
      <td style="text-align:left">word-break</td>
      <td style="text-align:left">
        <p>normal(&#x6D4F;&#x89C8;&#x5668;)</p>
        <p>break-all(&#x968F;&#x610F;)</p>
        <p>keep-all(&#x534A;&#x89D2;&#x7A7A;&#x683C;&#x6216;&#x8FDE;&#x5B57;&#x7B26;&#x5904;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">word-wrap</td>
      <td style="text-align:left">break-word(&#x5728;&#x957F;&#x5355;&#x8BCD;&#x6216; URL &#x5730;&#x5740;&#x5185;&#x90E8;&#x8FDB;&#x884C;&#x6362;&#x884C;)</td>
    </tr>
  </tbody>
</table>

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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Serif &#x5B57;&#x4F53;</th>
      <th style="text-align:left">&#x6587;&#x672C;&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Georgia, serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&quot;Palatino Linotype&quot;, &quot;Book Antiqua&quot;, Palatino, serif</td>
      <td
      style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&quot;Times New Roman&quot;, Times, serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">sans - serif&#x5B57;&#x4F53;</td>
      <td style="text-align:left">&#x6587;&#x672C;&#x793A;&#x4F8B;</td>
    </tr>
    <tr>
      <td style="text-align:left">Arial, Helvetica, sans-serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Arial Black, Gadget, sans-serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&quot;Comic Sans MS&quot;, cursive, sans-serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Impact, Charcoal, sans-serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&quot;Lucida Sans Unicode&quot;, &quot;Lucida Grande&quot;, sans-serif</td>
      <td
      style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Tahoma, Geneva, sans-serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&quot;Trebuchet MS&quot;, Helvetica, sans-serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Verdana, Geneva, sans-serif</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Monospace &#x5B57;&#x4F53;</td>
      <td style="text-align:left">&#x6587;&#x672C;&#x793A;&#x4F8B;</td>
    </tr>
    <tr>
      <td style="text-align:left">&quot;Courier New&quot;, Courier, monospace</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&quot;Lucida Console&quot;, Monaco, monospace</td>
      <td style="text-align:left">
        <p>&#x8FD9;&#x662F;&#x6807;&#x9898;</p>
        <p>This is a paragraph</p>
      </td>
    </tr>
  </tbody>
</table>


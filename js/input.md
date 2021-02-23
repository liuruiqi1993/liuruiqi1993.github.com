# input事件顺序

[https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/129](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/129)

* 输入到input框触发`input`事件
* 失去焦点后内容有改变触发`change`事件
* 识别到你开始使用中文输入法触发`compositionstart` 事件
* 未输入结束但还在输入中触发`compositionupdate`事件
* 输入完成（也就是我们回车或者选择了对应的文字插入到输入框的时刻）触发`compositionend`事件。

复制粘贴中文内容的时候不会触发`composition`事件, 用`onpaste`

参考vue源码对v-model的实现中，对输入中文的处理

```javascript
<input id='myinput'>

function jeiliu(timeout){
    var timer;
    function input(e){
    if(e.target.composing){
        return ;
    }
    if(timer){
        clearTimeout(timer);
    }
    timer = setTimeout(() => {
            console.log(e.target.value);
            timer = null;
        }, timeout);
    }
    return input;
}

function onCompositionStart(e){
    e.target.composing = true;
}
function onCompositionEnd(e){
    //console.log(e.target)
    e.target.composing = false;
    var event = document.createEvent('HTMLEvents');
    event.initEvent('input');
    e.target.dispatchEvent(event);
}
var input_dom = document.getElementById('myinput');
input_dom.addEventListener('input',jeiliu(1000));
input_dom.addEventListener('compositionstart',onCompositionStart);
input_dom.addEventListener('compositionend',onCompositionEnd);
```


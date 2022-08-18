# webpack
(作用域和闭包理解总结)[https://juejin.cn/post/6944609078186344484#heading-9]
## 私有变量
我们知道js中本没有私有变量的概念，但是我们可以借助闭包模拟实现：
```js
function privateObj(){
  var _privateStr="name";
  return {
    setStr(str){
      _privateStr=str;
    },
    getStr(){
      return _privateStr;
    },
  }
}
let obj=privateObj();
obj.setStr('new name');
obj.getStr();
```
这样我们就可以通过privateObj函数创建拥有_privateStr私有变量的对象了，这个的对象的_privateStr属性只能通过内部提供的setStr和getStr两个方法获取。

## 模块化
```js
var MyModule=(
    function(){
      var modules={};
      //定义模块
      function defined(name,deps,moduleFn){
         deps.forEach((dep,index)=>{
            deps[index]=modules[dep]; 
         })
         modules[name]=moduleFn.apply(moduleFn,deps);//为模块赋值，即模块生成函数返回的对象
      }
      //使用模块
      function require(name){
        return modules[name];
      }
      return {
        defined,
        require
      }
    }
  )()
//使用
MyModule.defined('bar',[],function(){
   var _name='bar_name'
   function showName(){
      console.log(_name)
   }
   function setName(name){
      _name = name
   }
   return {
     showName,
     setName
   }
})
MyModule.defined('foo',['bar'],function(bar){
   var _name='foo_name'
   function showBarName(){
      bar.showName()
   }
   function showName(){
      console.log(_name)
   }
   function setName(name){
      _name = name
   }
   return {
     showName,
     showBarName,
     setName
   }
})
MyModule.defined('all',['bar','foo'],function(bar,foo){
    return {
        bar,
        foo
    }
 })
var bar=MyModule.require('bar');
bar.setName('BAR');
var foo=MyModule.require('foo');
foo.setName('FOO');
foo.showName();
foo.showBarName();
var all=MyModule.require('all');
all.bar.showName()
all.foo.showName();


// FOO
// BAR
// BAR
// FOO
```

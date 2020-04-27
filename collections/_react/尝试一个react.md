---
layout: post
title: 尝试一个react
date: 2020-03-18
---

本菜鸡开工啦。昨天说要写Reactor。照着全家桶写吧  
（react + redux(状态管理) +react-router(路由) + axios + antd）  
应该囊括
1. 要有路由跳转
2. 要有表单提交，要有数据同步
3. 组件
4. axios可能就算了，没有条件。要不然考虑返回时拦截一下给假数据，正好学下mock怎么用，嘻，嘻

想一下要写什么
* nav bar（首页，搜索，个人）
* 首页tabbar，每切一下就换个列表
* 搜索+筛选条件，点击跳转tab
* 个人中心提交个人信息，查看成绩列表（假的）。
* 包含事件传递

### 过程记录
* tic-tac-toe
    * 照着react教程写，没有压力，练习事件传递
* 打地鼠
    * 照着[http://www.jq22.com/jquery-info15570](http://www.jq22.com/jquery-info15570)写
    * 地鼠square这个dom监听动画结束时调用reset，reset()里拿到的this是square dom并不是react 实例。应该写reset.bind(this)，call和apply会立即执行，而bind之后它还是一个方法， this指向该square组件，并可获得一个参数event。
    * 重置地鼠，去掉animation和重新添加animation之间要间隔出时间差，不然它就容易不动了。
    * mouseover 鼠标变锤子 闪来闪去  
    pointer-events: none;除了指示该元素不是鼠标事件的目标之外，值none表示鼠标事件“穿透”该元素并且指定该元素“下面”的任何东西。 [https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events](https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events)
    * 这本是二十几号开始的事情，地鼠有点卡壳了，刚好有新的小程序，现在小程序做到节点又不想捡起地鼠，心态崩了。先搁置去重新看VUE文档。
    * 

* 2048
* 计算器
* 猜拳


* 扫雷


### 不吐槽不舒服
1. 文档死活打不开，youtube倒是一点不卡，在翻和不翻之间来回倒腾，顶你个肺喔
2. 我还是觉得react写起来很麻烦。我以前有个同事总在吹vue不高级，react比较原生blah blah blah，uni-app牛皮多端blah blah blah。结果我自己用uni-app时要写二维码卡片分享。uni-app不能html2canvas，最后得自己一行行canvas写上去。如今写react也没觉得它高级，麻烦倒是麻烦不少，除了代码敲起来麻烦，文档也不好懂。老实讲，我觉得他只是在炫耀他会而已，男生的话不能信( ╯-_-)╯┴—┴ 
3. ant mobile 使用感不佳，把这个写完了下一步去研究element-ui和vant-ui怎么封的，然后把form，list，refresh，tab抄一遍。
---
layout: post
title: React笔记
---

文档： [https://create-react-app.dev/docs/](https://create-react-app.dev/docs/)

### 路由

#### 问题一：编程式导航，this.props.history 得到 undefined

* 路由组件 (经过路由匹配渲染的组件) 可以直接获取这些属性，而非路由组件就必须通过withRouter修饰后才能获取这些属性了

* 非路由组件需要路由参数时，使用withRouter给此组件传入路由参数，可以将react-router的 history、location、match 三个对象传入不是通过路由切换过来的组件的props对象上，使得被修饰的组件可以从属性中获取history,location,match

```
import React from "react";
import {withRouter} from "react-router-dom";
 
class MyComponent extends React.Component {
  ...
  myFunction() {
    this.props.history.push("/some/Path");
  }
  ...
}
export default withRouter(MyComponent);
```
#### 问题二：switch用法
路由变了，新组件没有挂载，去掉<switch>就可以跳了
因为switch需要搭配exact用
比如"/"，"/search"，"/account"，那么search和account可以视作是/的子路由，那么点击search或account还是会就近匹配到/。
```
<Router>
  <Switch>
    <Route exact path="/" component={Home} />
    <Route path="/search" component={Search} />
    <Route path="/account" component={Account} />
  </Switch>
</Router>
```

### 组件
#### [相对路径改绝对路径](https://create-react-app.dev/docs/importing-a-component/#absolute-imports)

```
// jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "include": ["src"]
}
```
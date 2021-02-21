# jekyll

记录用jekyll建博中遇到的麻烦，主要是墙
系统: mac Mojave
参考:

1. （中文文档）[http://jekyllcn.com/](http://jekyllcn.com/)
2. （英文文档）[https://jekyllrb.com](https://jekyllrb.com)
3. （参考三）[https://alligator.io/jekyll/](https://alligator.io/jekyll/)
4. （参考四：从无到有全过程，这不是我的重点）[https://blog.csdn.net/qq_19799765/article/details/80869363](https://blog.csdn.net/qq_19799765/article/details/80869363)和[https://zhuanlan.zhihu.com/p/28321740](https://zhuanlan.zhihu.com/p/28321740)

## 模版

图省事，从[https://github.com/jekyll/minima](https://github.com/jekyll/minima)拷了文件，想照着readme直接`bundle exec jekyll serve`运行，结果报错说不认识`bundle` 和 `jekyll`。

> You can find and preview themes on different galleries:
[jamstackthemes.dev](https://jamstackthemes.dev/ssg/jekyll/)
[jekyllthemes.org](http://jekyllthemes.org/)
[jekyllthemes.io](https://jekyllthemes.io/)
[jekyll-themes.com](https://jekyll-themes.com/)

## 安装bundle和jekyll

按照jekyll文档`gem install jekyll bundler`报错，最后找到原因

**一号墓坑: mac Ruby要 > 2.4.0**

> Jekyll requires Ruby > 2.4.0.macOS Catalina 10.15 comes with ruby 2.6.3, so you’re fine. If you’re running a previous macOS system, you’ll have to install a newer version of Ruby.

## 安装Ruby

[https://jekyllrb.com/docs/installation/](https://jekyllrb.com/docs/installation/)
试了半天才发现MAC教程里的 *With Homebrew*和*With rbenv*实际上用的相同的一句，至于是不是教程写错了，咱也不敢问。

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

**二号墓坑：网速是真的慢呀，十几K几K的下，怪你上网的时间不对**。

因为都是*With Homebrew*所以就照着*With Homebrew*教程走到黑。

``` c
brew install ruby

echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile

ruby -v
ruby 2.6.3p62 (2019-04-16 revision 67580)

# 如果需要升级gem，见https://gems.ruby-china.com/

gem install jekyll bundler

bundler install

bundle exec jekyll serve
```

这里就可以运行了，后面就都是自定义样式遇到的问题。

## 建立集合

jekyll默认把md文件放进_post中，通过site.posts去取。如果想分类，中文和英文文档里collection这一部分都一言难尽，请参考[https://alligator.io/jekyll/collections/](https://alligator.io/jekyll/collections/)

```yml
_config.yml

collections_dir: collections
collections:
  js:
    output: true
    title: Javasript
  record:
    output: true
    title: 记录
  css:
    output: true
    title: Css
  webpack:
    output: true
    title: Webpack
```

改完`_config.yml`重新运行项目。

```yml
// 文件结构

_layout
    default.html
    home.html
    list.html
    post.html
collections   (和_config.yml同一层)
    _css      (里面放md或子文件夹，注意名字里的_)
    _js       (里面放md或子文件夹，注意名字里的_)
    _record   (里面放md或子文件夹，注意名字里的_)
    _webpack  (里面放md或子文件夹，注意名字里的_)

```

上两部结束就能`site.js`，`site.css`，`site.record`这样取到数组。

![](image/20200316010138.jpg)

因为collections下有多个collection，所以我选择遍历`site.collections`。语法看[https://shopify.github.io/liquid/](https://shopify.github.io/liquid/)。
for 循环里加上`limit:3`表示最多展示3个。size是数组长度。

>`if post.menu == undefined`与第9行是我私人的判断条件，后面再解释。(起初这样写是因为不能像javascript这样`!post.menu`，然而写这篇文章时碰巧发现这一句还能这样写`unless post.menu`，见[Control flow](https://shopify.github.io/liquid/tags/control-flow/))

![](image/image20200316011222.jpg)

我希望实现的效果：
首页中展示collections（即前文_config.yml里定义的[css，js，record，webpack]）
每个collection最多展示2条，加上list.md所以limit是2。
点击collection title，就去列表页，展示collection下的post。
点击post title，就跳去详情页。

![](image/20200316013355.jpg)

详情页post.html，是拷下来的文件里写好的。
列表页list.html照着home.html改了一下，你以为这样就完了吗？
我在这里遇到了**3号墓坑，路由怎么都不对，`/collectionLabel/list`返回404**。

![](image/20200316014749.jpg)

我是怎么找着原因的呢？因为`/home`也是404，那么就可以肯定不是按寻常`/index.html`， `/home.html`这种思路去读。这就是写list.md的理由，每个collection下都有一个list.md，每个list.md都定义permalink。如此，当路由和permalink吻合时，就能找到对应的collection下的list.md，展示正确的列表页。

```md
_record下的list.md

---
layout: list             <!-- 指套用_layout中的list.html为模版 -->
permalink: /record/list
title: 记录
type: record
menu: true
---
```

同理每一篇博文post.md需要套用post.html最为模版，menu为假。日期也可以写在文件名上，如：`2011-12-31-new-years-eve-is-awesome.md`

```md
---
layout: post
date: 2020-03-15
---
```

这样建立集合存在的问题，`site.post`是按时间倒序排的，`site.js`呈现的是正序
为了实现最新在最上，用`assign my_array = collection.docs | sort: "date" | reverse`转一下

 **四号墓坑：_site文件没上传，结果scss样式没有编译，bootStrap的NAV栏切换也不生效，my bad**
 **五号墓坑：[添加目录Table of Contents](https://blog.webjeda.com/jekyll-toc/)**
 **六号墓坑：[添加目录collepse](http://t.hengwei.me/post/%E4%B8%BAjekyll%E5%8D%9A%E5%AE%A2%E6%B7%BB%E5%8A%A0%E7%9B%AE%E5%BD%95%E4%B8%8Escrollspy%E6%95%88%E6%9E%9C.html)**

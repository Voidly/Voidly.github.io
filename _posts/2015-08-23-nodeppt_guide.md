---
layout: post
title: "极客风的ppt制作软件-nodeppt使用指南"
description: 
modified: 2015-08-23
category: Tools
tags: [Tools,Nodeppt]
comments: true
mathjax: 

---
{% highlight linenos %}
{% endhighlight %}

在我的各种编辑工具都被markdown占领之后，几个月前，我又将制作PPT的软件转移到Markdown上来，这个开源的项目名为-nodeppt。

##为什么使用nodeppt

 * 基于GFM的markdown语法编写
 * 支持[html混排](#mixed-code)，再复杂的demo也可以做！
 * [导出网页](#export-html)或者[pdf](#export-pdf)更容易分享
 * 支持[20种转场动画](#transition)，可以设置单页动画
 * 支持多个皮肤：[colors](http://qdemo.sinaapp.com/)-[moon](http://qdemo.sinaapp.com/?theme=moon)-[blue](http://qdemo.sinaapp.com/?theme=blue)-[dark](http://qdemo.sinaapp.com/?theme=dark)-[green](http://qdemo.sinaapp.com/?theme=green)-[light](http://qdemo.sinaapp.com/?theme=light)
 * 支持单页背景图片
 * 多种模式：overview模式，[双屏模式](#postmessage)，[socket远程控制](#socket)，摇一摇换页，使用ipad/iphone控制翻页更酷哦~
 * 可以使用画板，**双屏同步画板**内容！可以使用note做备注
 * 支持语法高亮，自由选择[highlight样式](https://highlightjs.org/)
 * 可以单页ppt内部动画，单步动画
 * [支持进入/退出回调](#callback)，做在线demo很方便
 * 支持事件update函数，查看[demo](http://qdemo.sinaapp.com/#12)
 * zoom.js：alt+click

##安装

因为是基于nodejs的一个项目，因此需要使用npm进行安装。
{% highlight bash linenos %}
npm install -g nodeppt
{% endhighlight %}

至此安装结束。

##创建ppt

* markdown格式：

{% highlight bash linenos %}
nodeppt create ppt-name
{% endhighlight %}


* html格式：

{% highlight bash linenos %}
nodeppt create ppt-name.html
{% endhighlight %}
 or
{% highlight bash linenos %}
nodeppt create ppt-name.htm
{% endhighlight %}

##配置

{% highlight html linenos %}
title: 这是演讲的题目 
speaker: 演讲者名字 
url: 可以设置链接 
transition: 转场效果，例如：zoomin
files: 引入js和css的地址，如果有的话~自动放在页面底部
{% endhighlight %}

## subslide

subslide是在一页幻灯片中播放多个子页面，使用`subslide`标签包裹，子页面之间使用`======`间隔

{% highlight html linenos %}
[slide]

[subslide]
## 这是一个列表
---
* 上下左右方向键翻页
    * 列表支持渐显动画 {:&.moveIn}
    * 支持多级列表
    * 这个动画是moveIn
* 完全基于markdown语法哦
============
## 这是一个数字类型列表
---
1. 数字列表 {:&.rollIn}
2. 数字列表
3. 数字列表
[/subslide]
{% endhighlight %}

## theme 自定义
感觉默认的模板不符合新意？可以支持自定义模板，查看[theme.moon](https://github.com/ksky521/nodePPT/blob/master/assets/scss/theme.moon.scss)

自定义后的模板路径在markdown的设置里填写：

{% highlight html linenos %}
title: 这是演讲的题目
speaker: 演讲者名字
url: 可以设置链接
transition: 转场效果，例如：zoomin/cards/slide
files: /css/theme.moon.css
{% endhighlight %}

另外有：[colors](http://qdemo.sinaapp.com/)-[moon](http://qdemo.sinaapp.com/?theme=moon)-[blue](http://qdemo.sinaapp.com/?theme=blue)-[dark](http://qdemo.sinaapp.com/?theme=dark)-[green](http://qdemo.sinaapp.com/?theme=green)-[light](http://qdemo.sinaapp.com/?theme=light) 共六套自带皮肤可供选择

{% highlight html linenos %}
theme: moon
{% endhighlight %}
or url?theme=moon



## shell使用

### 启动

{% highlight bash linenos %}
# 获取帮助
nodeppt start -h
# 绑定端口
nodeppt start -p <port>
{% endhighlight %}

支持的转场动画包括：

 * kontext
 * vkontext
 * circle
 * earthquake
 * cards
 * glue
 * stick
 * move
 * newspaper
 * slide
 * slide2
 * slide3
 * horizontal3d
 * horizontal
 * vertical3d
 * zoomin
 * zoomout
 * pulse


如果设置单页动画，请参考下面的**[单页动画设置](#transition-page)**部分~

#### 分页
通过```[slide]```作为每页ppt的间隔，如果需要添加单页背景，使用下面的语法：

{% highlight html linenos %}
[slide style="background-image:url('/img/bg1.png')"]
# 这是个有背景的家伙
## 我是副标题
{% endhighlight %}

#### 单页ppt上下布局
{% highlight html linenos %}
[slide]
## 主页面样式
### ----是上下分界线
----
nodeppt是基于nodejs写的支持 **Markdown!** 语法的网页PPT

nodeppt：https://github.com/ksky521/nodePPT
{% endhighlight %}

#### 代码格式化
语法跟**Github Flavored Markdown** 一样~


#### 单条动画
使用方法：列表第一条加上 ` {:&.动画类型}``（注意空格）

{% highlight html linenos %}
* 上下左右方向键翻页
    * 列表支持渐显动画 {:&.moveIn}
    * 支持多级列表
    * 这个动画是moveIn
{% endhighlight %}

目前支持的单条动画效果包括：

* moveIn
* fadeIn
* bounceIn
* rollIn
* zoomIn

<a name="transition-page"></a>
#### 单页动画设置
在html文件，顶部 ```配置``` 可以设置全局转场动画，如果要设置单页的转场动画，可以通过下面的语法

{% highlight html linenos %}
[slide data-transition="vertical3d"]
## 这是一个vertical3d的动画
{% endhighlight %}

<a name="mixed-code"></a>
#### 插入html代码
如果需要完全diy自己的ppt内容，可以**直接使用** html标签，支持markdown和html混编。例如：

{% highlight html linenos %}
<div class="file-setting">
    <p>这是html</p>
</div>
<p id="css-demo">这是css样式</p>
<p>具体看下项目中 ppts/demo.html 代码</p>
<script>
    function testScriptTag(){

    }
    console.log(typeof testScriptTag);
</script>
<style>
#css-demo{
    color: red;
}
</style>
{% endhighlight %}

<a name="callback"></a>
#### 转场回调
前端的ppt，难免会在页面中演示一些demo，除了上面的插入html语法外，还提供了```incallback```和```outcallback```，分别用于：切入（切走）到当前ppt，执行的js函数名。例如：

{% highlight html linenos %}
[slide data-outcallback="outcallback" data-incallback="incallback"]
## 当进入此页，就执行incallback函数
## 当离开此页面，就执行outcallback函数
{% endhighlight %}

#### 表格实例
{% highlight html linenos %}
### 市面上主要的css预处理器：less\sass\stylus
---
 |less| sass | stylus
:-------|:------:|-------:|--------
环境 |js/nodejs | Ruby | nodejs
扩展名 | .less | .sass/.scss | .styl
特点 | 老牌，用户多，支持js解析 | 功能全，有成型框架，发展快 | 语法多样，小众
案例/框架 | [Bootstrap](http://getbootstrap.com/) | [compass](http://compass-style.org) [bourbon](http://bourbon.io) |
{% endhighlight %}

#### 插入iframe
使用```data-src```作为iframe的url，这样只有切换到当前页才会加载url内容~
{% highlight html linenos %}
<iframe data-src="http://www.baidu.com" src="about:blank;"></iframe>
{% endhighlight %}


#### 示例
类似下面的语法：(更多用法查看ppts/demo.html文件)
{% highlight html linenos %}
title: nodeppt markdown 演示
speaker: Theo Wang
url: https://github.com/ksky521/nodePPT
transition: zoomin

[slide]

# 封面样式
## h1是作为封面用的，内部的都用h2

[slide style="background-image:url('/img/bg1.png')"]

# 背景图片 {:&.flexbox.vleft}
## 使用方法：&#91;slide style="background-image:url('/img/bg1.png')"&#93;

[slide]

## 主页面样式
### ----是上下分界线
----

nodeppt是基于nodejs写的支持 **Markdown!** 语法的网页PPT

nodeppt：https://github.com/ksky521/nodePPT

[slide]

什么？这些功能还不够用？

极客模式：查看源码的nodeppt.js，相信你会找到牛逼的手机互动（摇一摇换页）功能

查看项目目录ppts获取更多帮助信息
{% endhighlight %}

更多demo，查看 ```ppts``` 目录的demo

### 查看帮助

{% highlight bash linenos %}
nodeppt -h
# 任何命令都可以输入-h查看帮助
nodeppt start -h
{% endhighlight %}

## demo演示 & 使用方法

 * 执行 ```nodeppt start```
 * 访问 [http://127.0.0.1:8080/](http://127.0.0.1:8080/)
 * 在线demo： http://qdemo.sinaapp.com/
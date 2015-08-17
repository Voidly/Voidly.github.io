---
layout: post
author: Voidly
title: "1分钟制作简单chrome扩展应用"
description: 
modified: 2015-08-15
category: [Chrome,Front-end]
tags: [Chrome]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

今天闲而无事，就玩起Chrome扩展应用来着，当然简单的扩展应用是很简单的，下面就进行简单的介绍。

##所需知识

* 一点点JSON
* 一点点HTML
* 一点点CSS
* 一点点JS

总而言之言而总之，做一个简单的Chrome扩展应用是基本不需要什么工作量的。不清楚以上知识的请移步到[w3cschool][1]。

##文件结构

其中主要文件包括:

* 图像文件images
* html文件
* 配置文件manifest.json
* js文件。

扩展被安装后，Chrome就会读取扩展中的manifest.json文件，这个文件的文件名固定为manifest.json，内容是按照一定格式描述的扩展相关信息，如扩展名称、版本、更新地址、请求的权限、扩展的UI界面入口等等。这样Chrome就可以知道在浏览器中如何呈现这个扩展，以及这个扩展如何同用户进行交互。

##需求介绍

由于是简单的Chrome扩展应用，因此仅仅是实现一个显示当前时间的扩展应用，当用户点击时钟图标时，会显示当前时间。

##代码实现

首先创建一个clock文件夹，包含以下内容：

* 一个images文件夹
* 一个js文件夹
* 一个manifest.json文件
* 一个popup.html文件。

首先，我们需要了解manifest.json都有哪些内容:

{% highlight json linenos %}
{
// 必须的字段
  "name": "My Extension",
  "version": "versionString",
  "manifest_version": 2,
  // 建议提供的字段
  "description": "A plain text description",
  "icons": { ... },
  "default_locale": "en",
  // 多选一，或者都不提供
  "browser_action": {...},
  "page_action": {...},
  "theme": {...},
  "app": {...},
  // 根据需要提供
  "background": {...},
  "chrome_url_overrides": {...},
  "content_scripts": [...],
  "content_security_policy": "policyString",
  "file_browser_handlers": [...],
  "homepage_url": "http://path/to/homepage",
  "incognito": "spanning" or "split",
  "intents": {...}
  "key": "publicKey",
  "minimum_chrome_version": "versionString",
  "nacl_modules": [...],
  "offline_enabled": true,
  "omnibox": { "keyword": "aString" },
  "options_page": "aFile.html",
  "permissions": [...],
  "plugins": [...],
  "requirements": {...},
  "update_url": "http://path/to/updateInfo.xml",
  "web_accessible_resources": [...]
} 
{% endhighlight %}

当然我们的manifest.json文件如下所示：

{% highlight json linenos %}
{
    "manifest_version": 2,
    "name": "我的时钟",
    "version": "1.0",
    "description": "我的第一个Chrome扩展",
    "icons": {
        "16": "images/icon16.png",
        "48": "images/icon48.png",
        "128": "images/icon128.png"
    },
    "browser_action": {
        "default_icon": {
            "19": "images/icon19.png",
            "38": "images/icon38.png"
        },
        "default_title": "我的时钟",
        "default_popup": "popup.html"
    }
}

{% endhighlight %}

可以看到，图片分为几种不同的，是因为更好的适应浏览器，图标文件都放在images文件夹下即可。`name`定义了扩展的名称，`version`定义了扩展的版本，`description`定义了扩展的描述，`icons`定义了扩展相关图标文件的位置。`version`的值最多可以是由三个圆点分为四段的版本号，每段只能是数字，每段数字不能大于65535且不能以0开头（可以是0，但不可以是0123），版本号段左侧为高位，比如1.0.2.0版本比1.0.0.1版本更高。每次更新扩展时，新的版本号必须比之前的版本号高。

`browser_action`指定扩展的图标放在Chrome的工具栏中，`browser_action`中的`default_icon`属性定义了相应图标文件的位置，`default_title`定义了当用户鼠标悬停于扩展图标上所显示的文字，`default_popup`则定义了当用户单击扩展图标时所显示页面的文件位置。

其中点击后会读取manifest.json文件后，然后调用popup.html文件，接下来就是实现html文件，这个也比较简单，仅仅是弹出一个窗口，然后显示时间，代码如下：

{% highlight html linenos %}
<html>
<head>
<style>
* {
    margin: 0;
    padding: 0;
}

body {
    width: 200px;
    height: 100px;
}

div {
    line-height: 100px;
    font-size: 42px;
    text-align: center;
}
</style>
</head>
<body>
<div id="clock_div"></div>
<script src="js/clock.js"></script>
</body>
</html>

{% endhighlight %}

其中就一个div用来显示窗口，而js获取时间之后每一秒钟刷新一次，再次进行显示，时间也就会被更新，具体代码如下：
{% highlight js linenos %}
function my_clock(el){
    var today=new Date();
    var h=today.getHours();
    var m=today.getMinutes();
    var s=today.getSeconds();
    m=m>=10?m:('0'+m);
    s=s>=10?s:('0'+s);
    el.innerHTML = h+":"+m+":"+s;
    setTimeout(function(){my_clock(el)}, 1000);
}

var clock_div = document.getElementById('clock_div');
my_clock(clock_div);

{% endhighlight %}

##效果

至此，一个Chrome扩展应用就完成了，下面就是将扩展打包，然后使用了，点击依次点击“菜单”-“更多工具”-“扩展程序”打开扩展程序页面（也可以直接在地址栏中输入chrome://extensions进入）,如下所示：

![][3]

勾选右上角的“开发者模式”，然后点击打包扩展程序，选择刚刚的clock文件夹(PS:第一次不用选择秘钥文件)，然后进行打包，打包之后你会获得两个文件，一个是*.crx,一个是\*.pem文件，其中第一个文件就是谷歌的扩展应用文件，只需要拖拽到扩展程序即可安装使用，效果如下：

![][4]

当然，你同样可以点击“加载正在开发的扩展程序”，选择扩展所在的文件夹，就可以在浏览器工具栏中看到我们的扩展了。

源码在[这里][9]。

##参考文献

> * [https://developer.chrome.com/extensions/getstarted][5]
> * [https://developer.chrome.com/extensions/overview][6]
> * [http://open.chrome.360.cn/extension_dev/overview.html][7]
> * [http://www.ituring.com.cn/minibook/950][8]



[1]:http://www.w3school.com.cn/
[2]:http://open.chrome.360.cn/extension_dev/manifest.html
[3]:https://github.com/Voidly/Img/blob/master/blog/chrome_extensions_1.png?raw=true
[4]:https://github.com/Voidly/Img/blob/master/blog/chrome_extensions_2.png?raw=true
[5]:https://developer.chrome.com/extensions/getstarted
[6]:https://developer.chrome.com/extensions/overview
[7]:http://open.chrome.360.cn/extension_dev/overview.html
[8]:http://www.ituring.com.cn/minibook/950
[9]:https://github.com/Voidly/chrome-extensions/tree/master/clock
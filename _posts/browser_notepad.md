title: 奇技淫巧之浏览器秒秒钟变编辑器
date: 2015-02-05 21:36:08
tags: [Editors,Translation]
categories: Editors
description: 只需一行代码，浏览器秒秒钟变成编辑器。
---

##由来
有时，我仅仅想输入一些乱码。仅仅想放空自己。用编辑器来输入这些胡言乱语会使我很苦恼，因为这样会弄乱我项目的工作区。（我很挑剔，我懂得）

所以我就这么做。自从我生活在有浏览器的地方，我就只需打开新的标签，然后在地址栏里输入下面的内容：

	data:text/html, <html contenteditable>

瞧，浏览器记事本。

##为什么代码会有效

你不需要记住它。它不是rocket science（哈哈，这里不是火箭科学，更不是那部电影，此处指艰深的学问以及复杂伤身的工作）。我们正在使用数据URI格式[Data URI's format](http://www.nczonline.net/blog/2009/10/27/data-uris-explained/ )并且告诉浏览器对html进行渲染(尝试一下"javascript:alert('Bazinga');")。html所说的内容是一种带有HTML的contenteditable（内容可编辑）的属性的HTML line。这仅仅在能识别这种属性的现代的浏览器上是有效的。点击编辑吧！

以上就是翻译的全部内容，原文戳[这里](https://coderwall.com/p/lhsrcq/one-line-browser-notepad)

效果图：

![](https://raw.githubusercontent.com/Voidly/Img/master/browser_editor.png)

##More

在作者发布这个消息之后，有很多的大神开始开动脑筋，绞尽脑汁，开始创作了各种碉堡的浏览器编辑器。下面这一款我比较喜欢的：

	data:text/html, <style type="text/css">.e{position:absolute;top:0;right:0;bottom:0;left:0;}</style><div class="e" id="editor"></div><script src="http://d1n0x3qji82z53.cloudfront.net/src-min-noconflict/ace.js" type="text/javascript" charset="utf-8"></script><script>var e=ace.edit("editor");e.setTheme("ace/theme/monokai");e.getSession().setMode("ace/mode/java");</script>

效果出众，最重要的是支持多种语言想用其它的语言的话，仅仅需要把`ace/mode/python`用下面的换掉即可：

	Markdown -> `ace/mode/markdown`
	Python -> `ace/mode/ruby`
	C/C++ -> `ace/mode/c_cpp`
	Javscript -> `ace/mode/javascript`
	Java -> `ace/mode/java`
	Scala- -> `ace/mode/scala`
	CoffeeScript -> `ace/mode/coffee`
	and 
	css, html, php, latex, 
	tex, sh, sql, lua, clojure, dart, typescript, go, groovy, json, jsp, less, lisp, 
	lucene, perl, powershell, scss, textile, xml, yaml, xquery, liquid, diff and many more...

更碉堡的是，主题也可以换的，仅仅把`ace/theme/monokai`用下面的换掉即可


	Eclipse -> ace/theme/eclipse
	GitHub -> ace/theme/github
	TextMate -> ace/theme/textmate
	and 
	ambiance, dawn, chaos, chrome, dreamweaver, xcode, vibrant_ink, solarized_dark, solarized_light, tomorrow, tomorrow_night, tomorrow_night_blue, 
	twilight, tomorrow_night_eighties, pastel_on_dark and many more..

如果你想用Markdown的话用下面的代码即可

	data:text/html,<style type="text/css">.e{position:absolute;top:0;right:50%;bottom:0;left:0;} .c{position:absolute;overflow:auto;top:0;right:0;bottom:0;left:50%;}</style><div class="e" id="editor"></div><div class="c"></div><script src="http://d1n0x3qji82z53.cloudfront.net/src-min-noconflict/ace.js" type="text/javascript" charset="utf-8"></script><script src="http://cdnjs.cloudflare.com/ajax/libs/showdown/0.3.1/showdown.min.js"></script><script> function showResult(e){consoleEl.innerHTML=e}var e=ace.edit("editor");e.setTheme("ace/theme/monokai");e.getSession().setMode("ace/mode/markdown");var consoleEl=document.getElementsByClassName("c")[0];var converter=new Showdown.converter;e.commands.addCommand({name:"markdown",bindKey:{win:"Ctrl-M",mac:"Command-M"},exec:function(t){var n=e.getSession().getMode().$id;if(n=="ace/mode/markdown"){showResult(converter.makeHtml(t.getValue()))}},readOnly:true})</script>

还有一个比较好玩的，是一个在编辑过程中会变色的，大家可以试试：

	data:text/html, <html><head><link href='http://fonts.googleapis.com/css?family=Open+Sans' rel='stylesheet' type='text/css'><style type="text/css"> html { font-family: "Open Sans" } * { -webkit-transition: all linear 1s; }</style><script>window.onload=function(){var e=false;var t=0;setInterval(function(){if(!e){t=Math.round(Math.max(0,t-Math.max(t/3,1)))}var n=(255-t*2).toString(16);document.body.style.backgroundColor="#ff"+n+""+n},1e3);var n=null;document.onkeydown=function(){t=Math.min(128,t+2);e=true;clearTimeout(n);n=setTimeout(function(){e=false},1500)}}</script></head><body contenteditable style="font-size:2rem;line-height:1.4;max-width:60rem;margin:0 auto;padding:4rem;">


另外我已经将代码放在github上了，欢迎Fork。[浏览器-编辑器](https://github.com/Voidly/browser-notepad)

最后附上一张Python编辑器的图：

![](https://raw.githubusercontent.com/Voidly/Img/master/python.png)

---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**
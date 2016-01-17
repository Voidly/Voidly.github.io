---
layout: post
title: "前端之jQuery拾遗"
description: 
modified: 2016-01-17
category: front-end
tags: [front-end,jQuery]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

前不久看的一些jQuery知识，在此进行一些简单的记录。

##一些基础

####window.onload与$(document).ready()对比

- window.onload

    - 必须等网页中所有内容加载完（包含图片）之后执行

    - 不能同时写多个window.onload

- $(document).ready()
    - DOM结构绘制完毕后执行，DOM关联元素并未加载完毕

    - 能同时写多个
    
####事件冒泡

   - 如果一个元素嵌套在另一个元素里，并且都被绑定了click事件，那么内层的被点击之后，会按照从里到外的顺序依次触发

   - 停止事件冒泡方法：

       - 使用事件对象

           - $(‘#span’).bind(‘click’, function(event){ event.stopPropagation(); })

   - 阻止事件默认行为:如单击超链接跳转，单击表单提交
       - event.preventDefault();

   - 停止冒泡，阻止事件默认行为可以都使用
       - return false;

   - 事件捕获顺序与事件冒泡顺序相反

##jQuery性能优化

- 使用最新版的jQuery库

- 使用合适的选择器

    - $(“#id")

    - $(“p”)、$(“div")

    - $(“.class")

    - $(“[attribute=value]")

    - $(“:hidden")
    - PS:尽量使用ID选择器；尽量给选择器指定上下文

- 缓存对象

- 循环时的DOM操作

    - 将循环的对象存储在列表，然后再进行DOM操作，减少DOM操作

- 数组方式使用jQuery对象

- 事件处理

    - 使用事件监听

        - $(“#id).on(“click”, “td”.function(){})

- 将你的代码转换为jQuery插件

- 使用join()来拼接字符串

- 合理使用HTML5的Data属性

- 尽量使用原生的js方法

- 压缩JavaScript

##jQuery技巧


- 禁用右键菜单

{% highlight js linenos %}
$(document).ready(function(){
$(document).bind(“contextmenu”,function(e){
return false;
  });
});
{% endhighlight %}



- 新窗口打开

{% highlight js linenos %}
  $(‘a[href^=“http://"]’).attr(“target”, “_blank");
{% endhighlight %}

- 判断浏览器类型

{% highlight js linenos %}
$.browser.chrome && $.browser.version >= xxx;
{% endhighlight %}


- 输入框文字获取和失去焦点

{% highlight js linenos %}
input.focus()
input.blur()
{% endhighlight %}

- 返回头部滑动动画

{% highlight js linenos %}
jQuery.fn.scrollTo = function(speed) {
    var targetOffset = $(this).offset().top;
    $('html,body').stop().animate({scrollTop: targetOffset}, speed);
    return this;
};
// use
$("#goheader").click(function(){
    $("body").scrollTo(500);
    return false;
});
{% endhighlight %}

- 获取鼠标位置

{% highlight js linenos %}
    e.pageX和e.pageY
{% endhighlight %}    

- 判断元素是否存在


    - 使用length

- 回车提交表单

{% highlight js linenos %}
  $(document).ready(function() {
       $("input").keyup(function(e){
              if(e.which=="13"){
                 alert("回车提交!")
              }
          })
  });
{% endhighlight %}    

- 获取选中的下拉框

{% highlight js linenos %}
function getObj(){
    var $obj = $('#someElement').find('option:selected');
    alert( $obj.val() );
}
{% endhighlight %} 

- 个性化链接

- 本地存储
---
layout: post
title: "python之PIL模块小试"
description: 
modified: 2015-08-09
category: python
tags: [Python,PIL]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

今天无意间看到了python的PIL-Python Imaging Library，于是便想搞个验证码的图片玩玩，首先自然是安装PIL咯，有关安装的pip和easy_install两种方式不明的同学请戳[Python: What's PyPi, pip, easy_install, setuptools?][2]和[pip vs easy_install][3]。

##安装出BUG

不过出师不利啊，安装就遇到了问题：

```
_imagingft.c:73:10: fatal error: 'freetype/fterrors.h' file not found #include <freetype/fterrors.h> ^ 1 error generated. error: Setup script exited with error: command 'cc' failed with exit status 1
```

谷歌了下，解决方案是创建一个符号链接：

{% highlight bash %}
ln -s /usr/local/include/freetype2 /usr/local/include/freetype
{% endhighlight %}

问题得以解决，接下来就是用Image模块来做一个验证码的图咯。

##实现

下面就开始用python的Image模块实现图片验证码：

{% highlight python linenos %}
# -*- coding:utf-8 -*-

import Image, ImageDraw, ImageFont, ImageFilter
import random

# 随机字母:
def rndChar():
    return chr(random.randint(65, 90))

# 随机颜色1:
def rndColor():
    return (random.randint(64, 255), random.randint(64, 255), random.randint(64, 255))

# 随机颜色2:
def rndColor2():
    return (random.randint(32, 127), random.randint(32, 127), random.randint(32, 127))

# 240 x 60:
width = 60 * 4
height = 60
image = Image.new('RGB', (width, height), (255, 255, 255))
# 创建Font对象:
font = ImageFont.truetype('/Library/Fonts/Andale Mono.ttf', 36)
# 创建Draw对象:
draw = ImageDraw.Draw(image)
# 填充每个像素:
for x in range(width):
    for y in range(height):
        draw.point((x, y), fill=rndColor())
# 输出文字:
for t in range(4):
    draw.text((60 * t + 10, 10), rndChar(), font=font, fill=rndColor2())
# 模糊:
image = image.filter(ImageFilter.BLUR)
image.save('code.jpg', 'jpeg');
{% endhighlight %}

##效果

![][1]

##小结

如果你觉得不过瘾，可以去[这里][4]深入了解图像处理的各种功能，包括翻转、剪切、颜色变换 and the like。


[1]:https://github.com/Voidly/Img/blob/master/blog/code.jpg?raw=true
[2]:http://xahlee.info/python/python_whats_pip_easyinstall_setuptools.html
[3]:https://packaging.python.org/en/latest/pip_easy_install.html
[4]:http://effbot.org/imagingbook/introduction.htm
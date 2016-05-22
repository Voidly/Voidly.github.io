---
layout: post
title: "使用property装饰器达到setter/getter效果"
description: 
modified: 2016-04-16
category: python
tags: [python, property]
comments: true
mathjax: 
---

曾在`java`使用中比较常见到`getter`以及`setter`方法,而在`python`中这个却不常见到,最近见到了一种比较`pythonic`的写法，是使用`property`装饰器来实现`getter`以及`setter`的效果。

## 示例


```python
# -*- coding: utf-8 -*-

class Student(object):
    
    @ property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        self._score = value


if __name__ == "__main__":
    s = Student()
    s.score = 4
    print s.score
    s.score = 5
    print s.score

```

当然，你可以使用`property`来实现只读属性。



---
layout: post
title: "python的stdout与stdin"
description: 
modified: 2016-05-22
category: python
tags: [python,stdout]
comments: true
mathjax: 
---

## sys.stout与print

```python
print obj
```

调用的其实是

```python
sys.stdout.write(obj)
```

## sys.stdin与raw_input

```python
hi = raw_input("hello")
```

等价于

```python
print "hello"
hi = sys.stdin.readline()[:-1]  #-1去掉"\n"
```

## 示例

最后，附上一份同时输出到控制台以及文件的代码:

```python
import sys

class __redirection__:
    def __init__(self):
        self.buff=''
        self.__console__=sys.stdout

    def write(self, output_stream):
        self.buff+=output_stream

    def to_console(self):
        sys.stdout=self.__console__
        print self.buff

    def to_file(self, file_path):
        f=open(file_path,'w')
        sys.stdout=f
        print self.buff
        f.close()

    def flush(self):
        self.buff=''

    def reset(self):
        sys.stdout=self.__console__

if __name__=="__main__":
    # redirection
    r_obj=__redirection__()
    sys.stdout=r_obj

    # get output stream
    print 'hello'
    print 'there'

    # redirect to console
    r_obj.to_console()

    # redirect to file
    r_obj.to_file('out.log')

    # flush buffer
    r_obj.flush()

    # reset
    r_obj.reset()
```

## 参考

> * https://docs.python.org/2/library/sys.html



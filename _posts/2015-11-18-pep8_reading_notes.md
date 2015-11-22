---
layout: post
title: "pep8阅读笔记"
description: 
modified: 2015-11-18
category: Python
tags: [Python,Programming Style]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

一直以来比较认同“代码不是写给机器看的，而是写给人看的”这句话，因此如何编写易于阅读的代码就就成了一个问题。一直都听闻pep8是Pythonista编程风格指向，但由于懒而未曾看过，最近抽空翻阅了一下pep，发现好多规范其实已经在使用了，现将其中部分摘录于此，以作备忘。主要包含代码布局以及注释两大部分。

##代码布局

- 缩进4个空格

    - 不要混合使用 tab 和空格
- Tab 还是空格 (Tabs or Spaces)？

    - 强烈推荐只使用空格

- 最大行宽 (Maximum Line Length)

    - 限制所有行的最大行宽为 79 字符。

    - 周围仍然有许多设备被限制在每行 80 字符；而且，窗口限制在 80 个字符，使将多 个窗口并排放置成为可能。在这些设备上使用默认的折叠 (wrapping) 方式看起来有 点丑陋。 因此，请将所有行限制在最大 79 字符。对顺序排放的大块文本 (文档字符 串或注释 (docstrings or comments))，推荐将长度限制在 72 字符。折叠长行的首选方法是使用 Python 支持的圆括号、方括号 (brackets) 和花括号 (braces) 内的行延续。如果需要，你可以在表达式周围增加一对额外的圆括号，但是 有时使用反斜杠看起来更好。确认恰当地缩进了延续的行。

- 空行 (Blank Lines)

    - 用两行空行分割顶层函数和类的定义。

    - 类内方法的定义用单个空行分割。

    - 额外的空行可被用于 (保守的 (sparingly)) 分割相关函数群 (groups of related functions)。在一组相关的单句 (related one-liners) 中间可以省略空行 (例如一组哑元 (dummy implementations))。

    - 在函数中使用空行时，请谨慎的用于表示一个逻辑段落 (logical sections)。

- 编码 (Encodings) (PEP 263)

    - Python 核心发布中的代码应该始终使用 ASCII 或 Latin-1 编码(又名ISO-8859-1)。

    - 使用ASCII的文件不必有译码 cookie (coding cookie)。 Latin-1 仅当注释或文档字 符串涉及作者名字需要 Latin-1 时才被使用；另外使用 \x 转义字符是在字符串中包 含非 ASCII 数据的首选方法。

- 导入 (Imports)

    - 通常应该在单独的行中导入
    - Imports 通常被放置在文件的顶部，仅在模块注释和文档字符串之后，在模块的全局变量和常量之前。
    - Imports应该按照如下顺序成组安放

        - 标准库的导入

        - 相关的第三方包的导入

        - 本地应用/库的特定导入

    - 每组导入之间放置一个空行。

    - 把任何相关 all 说明的放在 imports 之后

    - 对所有导入总是使用包的绝对路径

- 避免在下述情形中使用无关的空格

    -  紧挨着圆括号、方括号和花括号
    -  紧贴在逗号、分号或冒号前
    -  紧贴着函数调用的参数列表前的开式括号
    -  紧贴在索引或切片 (indexing or slicing) 开始的开式括号前

    - 在赋值 (或其他) 运算符周围的用于和其他语句对齐的一个以上的空格

- 其他建议 (Other Recommendations)

    - 始终在这些二元运算符两边放置一个空格

        - assignment (=), augmented assignment (+=, -= etc.), comparisons (==, <, >, !=, <>, <=, >=, in, not in, is, is not), Booleans (and, or, not).

    - 在数学运算符两边使用空格

    - 不要在用于指定关键字参数 (keyword argument) 或默认参数值的 '=' 号周围使用空格
    -  复合语句 (Compound statements) (多条语句写在同一行) 一般不推荐
    
##注释
 
 - 同代码不一致的注释比没注释更差。当代码修改时，始终优先更新注释！

- 注释应该是完整的句子。如果注释是一个短语或句子，首字母应该大写，除非它是一 个以小写字母开头的标识符 (永远不要修改标识符的大小写)。

- 如果注释很短，可以省略末尾的句号。

- 非英语国家的 Python 程序员：请用英语书写你的注释

- 注释块 (Block Comments)

    - 注释块通常应用于跟随其后的一些 (或者全部) 代码，并和这些代码有着相同的缩进 层次。注释块中每行以 '#' 和一个空格开始 (除非它是注释内的缩进文本)。

    - 注释块内的段落以仅含单个 '#' 的行分割

- 行内注释 (Inline Comments)

- 文档字符串 (Documentation Strings)

    - 为所有公共模块、函数、类和方法书写文档字符串。文档字符串对非公开的方法不是必要的，但你应该有一条注释来描述这个方法做什么；这条注释应该出现在 "def" 行之后。

    - 一定注意，多行文档字符串结尾的 """ 应该单独成行，并推荐在其前加一空行，

- 版本注记 (Version Bookkeeping)

    - 如果你必须把 Subversion、CVS、or RCS crud 包含在你的源文件中，按如下做：version = "$Revision: 54700 $"

- 命名约定 (Naming Conventions)

    - 在 CapWords 中使用缩写，需要把缩写的所有字母大写。故 HTTPServerError 比 HttpServerError 更好。

    - **_避免采用的名字 (Names to Avoid)_**

        - **决不要用字符 'l' (小写字母 el)，'O' (大写字母 oh)，或 'I' (大写字母 eye) 作为单个字符的变量名。**

    - **包和模块名 (Package and Module Names)

        - 模块名应该是简短的、全部小写的名字。可以在模块名中使用下划线以提高可读性 。Python 包名也应该是简短的、全部小写的名字，尽管不推荐使用下划线。))

    - 类名 (Class Names)

        - 几乎没有例外，类名使用首字母大写单词串 (CapWords) 的约定。 内部使用的类使用一个额外的前导下划线。

    - 异常名 (Exception Names)

        - 因为异常应该是类，故类命名约定也适用于异常。然而，你应该对异常名添加后缀 "Error" (如果该异常的确是一个错误)。

    - 全局变量名 (Global Variable Names)

    - 函数名 (Function Names)

        - 函数名应该为小写，必要时可用下划线分隔单词以增加可读性。

    - 函数和方法的参数 (Function and method arguments)

        - 对实例的方法，总是用 'self' 做第一个参数。

        - 对类的方法，总是用 'cls' 做第一个参数。

        - 如果函数的参数名与保留关键字冲突，在参数名后加一个下划线，比用缩写、错误 的拼写要好。因此 "print" 比 "prnt" 好。(也许使用同义词来避免冲突更好。)

    - 方法名和实例变量 (Method Names and Instance Variables)

        - 采用函数命名规则：小写单词，必要时可用下划线分隔单词以增加可读性。

- 继承的设计 (Designing for inheritance)

    - 总是确定类的方法和实例变量 (统称为属性) 是否应该被公开或者不公开。如果有 疑问，选择不公开；今后把其改为公开比把一个公开属性改为非公开要容易。

- 设计建议 (Programming Recommendations)

    - 与 None 之类的单件比较，应该总是用 'is' or 'is not'，绝不要用等号操作符。
    -  使用基于类的异常。

    - 在捕获异常时，尽可能写出明确的异常，而不是使用空的 'except:' 子句。

    - 使用 ''.startswith() and ''.endswith() 代替字符串切片，来检查前缀和后缀。

    - 对象类型的比较应该始终用 isinstance() 代替直接比较类型。

    - 对序列 (字符串 (strings)，列表 (lists)，元组 (tuples))，使用空序列为假

    - 不要书写依赖于有意义的后置空白字符的文本字符串。

    - 不要用 == 来把布尔值与 True 或 False 进行比较。
    
    
##参考

> https://code.google.com/p/zhong-wiki/wiki/PEP8
> https://www.python.org/dev/peps/pep-0008/
---
layout: post
title: "python之argparse模块"
description: 
modified: 2016-02-20
category: Python
tags: [Python, Argparse]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

有些时候我们不得不编写一些命令行工具，需要从外部传入进来一些参数，并对参数做一些解析工作，然后根据不同的参数做不同的操作，此时，`argparse`模块就派上了用场，我们只需要按照要求定义好各种参数，然后`argparse`将负责如何从`sys.argv`中解析出这些参数。`argparse`模块还会自动生成帮助和使用信息并且当用户赋给程序非法的参数时产生错误信息。

##创建解析器
    
####示例
{% highlight python linenos %}
parser = argparse.ArgumentParser(description='Process some integers.')
{% endhighlight %}

####类方法
{% highlight python linenos %}
class argparse.ArgumentParser(prog=None, usage=None, description=None, epilog=None, parents=[], formatter_class=argparse.HelpFormatter, prefix_chars='-', fromfile_prefix_chars=None, argument_default=None, conflict_handler='error', add_help=True)
{% endhighlight %}

####类参数解析


- 创建一个新的ArgumentParser对象。所有的参数应该以关键字参数传递。下面有对每个参数各自详细的描述，但是简短地讲它们是：
- prog - 程序的名字（默认：sys.argv[0]）
- usage - 描述程序用法的字符串（默认：从解析器的参数生成）
- description - 参数帮助信息之前的文本（默认：空）
- epilog - 参数帮助信息之后的文本（默认：空）
- parents - ArgumentParser 对象的一个列表，这些对象的参数应该包括进去
- formatter_class - 定制化帮助信息的类
- prefix_chars - 可选参数的前缀字符集（默认：‘-‘）
- fromfile_prefix_chars - 额外的参数应该读取的文件的前缀字符集（默认：None）
- argument_default - 参数的全局默认值（默认：None）
- conflict_handler - 解决冲突的可选参数的策略（通常没有必要）
- add_help - 给解析器添加-h/–help 选项（默认：True）


##添加参数

####类方法

{% highlight python linenos %}
ArgumentParser.add_argument(name or flags...[, action][, nargs][, const][, default][, type][, choices][, required][, help][, metavar][, dest])
{% endhighlight %}

####参数解析

- name or flags - 选项字符串的名字或者列表，例如foo 或者-f, --foo。
- action - 在命令行遇到该参数时采取的基本动作类型。
    - 'store'
        - - 只是保存参数的值。这是默认的动作。
    - 'store_const'
        - - 保存由const关键字参数指出的值。（注意const关键字参数默认是几乎没有帮助的None。）'store_const'动作最常用于指定某种标记的可选参数。
        - parser.add_argument('--foo', action='store_const', const=42)
    - 'store_true'和'store_false'
        - - 它们是'store_const' 的特殊情形，分别用于保存值True和False。另外，它们分别会创建默认值False 和True。
    - 'append'
        - - 保存一个列表，并将每个参数值附加在列表的后面。这对于允许指定多次的选项很有帮助。
        
{% highlight python linenos %}
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', action='append')
>>> parser.parse_args('--foo 1 --foo 2'.split())
Namespace(foo=['1', '2'])
{% endhighlight %}
    
   - 'count'
        - - 计算关键字参数出现的次数。例如，这可用于增加详细的级别：

{% highlight python linenos %}
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--verbose', '-v', action='count')
>>> parser.parse_args('-vvv'.split())
Namespace(verbose=3)
{% endhighlight %}
       
   - 'version'
        - - 它期待version=参数出现在add_argument()调用中，在调用时打印出版本信息并退出：

{% highlight python linenos %}
>>> import argparse
>>> parser = argparse.ArgumentParser(prog='PROG')
>>> parser.add_argument('--version', action='version', version='%(prog)s 2.0')
>>> parser.parse_args(['--version'])
PROG 2.0
{% endhighlight %}
 

- nargs - 应该读取的命令行参数数目。
    - N（一个整数）。

        - 命令行中的N个参数将被一起收集在一个列表中。
    - '?'。
        - 如果有的话就从命令行读取一个参数并生成一个元素。如果没有对应的命令行参数，则产生一个来自default的值。注意，对于可选参数，有另外一种情况 - 有选项字符串但是后面没有跟随命令行参数。在这种情况下，将生成一个来自const的值。
        - nargs='?'的一种更常见的用法是允许可选的输入和输出文件：

{% highlight python linenos %}
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('infile', nargs='?', type=argparse.FileType('r'),
...                     default=sys.stdin)
>>> parser.add_argument('outfile', nargs='?', type=argparse.FileType('w'),
...                     default=sys.stdout)
>>> parser.parse_args(['input.txt', 'output.txt'])
Namespace(infile=<open file 'input.txt', mode 'r' at 0x...>,
          outfile=<open file 'output.txt', mode 'w' at 0x...>)
>>> parser.parse_args([])
Namespace(infile=<open file '<stdin>', mode 'r' at 0x...>,
          outfile=<open file '<stdout>', mode 'w' at 0x...>)
{% endhighlight %}
          
   - '*'。

       - 出现的所有命令行参数都被收集到一个列表中。注意，一般情况下具有多个带有nargs='*'的位置参数是不合理的，但是多个带有nargs='*'的可选参数是可能的。
    - '+'。

     - 和'*'一样，出现的所有命令行参数都被收集到一个列表中。除此之外，如果没有至少出现一个命令行参数将会产生一个错误信息。
    - argparse.REMAINDER.

        - 所有剩余的命令行参数都被收集到一个列表中。这通常用于命令行工具分发命令到其它命令行工具：

{% highlight python linenos %}
>>> parser = argparse.ArgumentParser(prog='PROG')
>>> parser.add_argument('--foo')
>>> parser.add_argument('command')
>>> parser.add_argument('args', nargs=argparse.REMAINDER)
>>> print parser.parse_args('--foo B cmd --arg1 XX ZZ'.split())
Namespace(args=['--arg1', 'XX', 'ZZ'], command='cmd', foo='B')
{% endhighlight %}
        
- const - 某些action和nargs选项要求的常数值。
- default - 如果命令行中没有出现该参数时的默认值。
- type - 命令行参数应该被转换成的类型。

    - 默认情况下，ArgumentParser对象以简单字符串方式读入命令行参数。然而，很多时候命令行字符串应该被解释成另外一种类型，例如浮点数或者整数。add_argument()的type关键字参数允许任意必要的类型检查并作类型转换。常见的内建类型和函数可以直接用作type参数的值：

{% highlight python linenos %}
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('foo', type=int)
>>> parser.add_argument('bar', type=file)
>>> parser.parse_args('2 temp.txt'.split())
Namespace(bar=<open file 'temp.txt', mode 'r' at 0x...>, foo=2)
{% endhighlight %}
    
- choices - 参数可允许的值的一个容器。

    - 某些命令行参数应该从一个受限的集合中选择。这种情况的处理可以通过传递一个容器对象作为choices关键字参数给add_argument()。当解析命令行时，将检查参数的值，如果参数不是一个可接受的值则显示一个错误信息：

{% highlight python linenos %}
>>> parser = argparse.ArgumentParser(prog='game.py')
>>> parser.add_argument('move', choices=['rock', 'paper', 'scissors'])
>>> parser.parse_args(['rock'])
Namespace(move='rock')
>>> parser.parse_args(['fire'])
usage: game.py [-h] {rock,paper,scissors}
game.py: error: argument move: invalid choice: 'fire' (choose from 'rock',
'paper', 'scissors')
{% endhighlight %}
      
- required - 该命令行选项是否可以省略（只针对可选参数）。
- help - 参数的简短描述。

    - 可用的指示符包括程序的名称%(prog)s以及大部分add_argument()的关键字参数，例如%(default)s、%(type)s等：
- metavar - 参数在帮助信息中的名字。
- dest - 给parse_args()返回的对象要添加的属性名称。


##添加子命令

许多程序将它们的功能分成多个子命令，例如，svn可以调用如svn checkout、svn update和svn commit子命令。在程序实现的几个不同的功能要求不同类型的命令行参数时，以这种方式分割程序的功能是特别好的主意。ArgumentParser支持用add_subparsers()方法创建这种子命令。add_subparsers() 方法的调用通常不带参数并返回一个特殊的动作对象。该对象只有一个方法add_parser()，它的参数是一个命令行名字和任意ArgumentParser构造器，返回的是一个可以像平常一样修改的ArgumentParser对象。

####类方法

{% highlight python linenos %}
ArgumentParser.add_subparsers([title][, description][, prog][, parser_class][, action][, option_string][, dest][, help][, metavar])
{% endhighlight %}
 
####参数说明

- title - 在输出的帮助中子解析器组的标题；默认情况下，如果提供description参数则为“subcommands”，否则使用位置参数的标题
- description - 在输出的帮助中子解析器组的描述，默认为None
- prog - 与子命令的帮助一起显示的使用帮助信息，默认为程序的名字和子解析器参数之前的所有位置参数
- parser_class - 用于创建子解析器实例的类，默认为当前的解析器（例如ArgumentParser）
- dest - 子命令的名字应该存储的属性名称；默认为None且不存储任何值
- help - 在输出的帮助中子解析器中的帮助信息，默认为None
- metavar - 在帮助中表示可用的子命令的字符串；默认为None并以{cmd1, cmd2, ..}的形式表示子命令


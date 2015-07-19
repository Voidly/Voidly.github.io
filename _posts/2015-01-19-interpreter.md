---
title: 设计模式之第5章-解释器模式(Java实现)
layout: post
date: 2015-01-19 12:12:16
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---
　“开个商店好麻烦，做个收单的系统，发现类的方法好多。”“真是的，不就是简单的四则运算，这都不会！”你说你会啊。来来来，你把以下的方法用代码写出来：

* a+b+c+d
* a+b-c
* a-b+c
* a+b
* a-e
　　、、、

这个就是最简单的一些商店的系统，当然了，这里仅仅包含加减，这个时候就需要我-解释器出马了。

##解释器模式之自我介绍

在你被给定一个语言，定义它的文法的一种表示，并定义一个一个解释器，用来解释语言中的句子。简单的说就是按照规定语法进行解析的方案，在现在的项目中使用比较少。有关我的定义如下：Given a language,define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.意思就是说：给定一门语言，定义它的文法的一种表示，并定义一个解释器，该解释器使用该表示来解释语言中的句子。通用的类图看下面：

![](http://images.cnitblog.com/blog/666211/201501/181652078865845.jpg)

* AbstractExpression-----抽象表达式

具体的解释任务由各个实现类完成，具体的解释器分别由TerminalExpression和NoterminalExpression完成。

* TerminalExpression----终结符表达式

实现与文法中的元素相关联的解释操作。通常一个解释器模式中只有一个终结符表达式，但是为了对应多个终结符，会有多个实例。

* NoterminalExpression--非终结符表达式

文法中的每条规则对应一个非终结符表达式，每个符号都需要维护一个AbstractExpression类型的实例变量。并且为文法中的非终结符实现解释操作。

* Context---上下文

包含解释器之外的一些个全局信息。

* Client---客户

构建表示该文法定义的语言中的一个特定句子的抽象语法树，可以调用解释操作。

##解释器方法之自我分析

嘛，我的缺点不是一般的多呢：

* 会引起类的膨胀，每个语法都要产生一个非终结符表达式，语法规则比较复杂时，可能产生大量的类文件，不易于维护。
* 采用递归的调用方法，导致调试比较复杂。
* 由于使用大量循环和递归，所以效率自然就低了。

优点：

* 扩展性很好，修改语法规则只要修改相应的非终结符表达式即可，若扩展语法，只要增加非终结符类就可以了。

##解释器之实现

具体实现你造不造？那我就拿简单的加减法来做个栗子。

AbstractExpression抽象类如下所示：

	public abstract class Expression{
	    //解析公式和数值，其中var中的key值是公式中的参数，value是具体的数字
	    public abstract int interpreter(HashMap<String,Integer> var);
	}

抽象类很简单，就一个方法interpreter负责对传递进来的参数和值进行解析和匹配，其中输入参数为HashMap类型，key值为模型中的参数，如a、b、c等，value为运算时取得的具体数字。变量解析器代码如下：

	public class varExpression extends Expression{
	    private String key;
	    public varExpression(String key){
	        this.key = key;
	    }
	
	    //从map中取key
	    public int interpreter(HashMap<String, Intrger> var){
	        return var.get(this.key);
	    }
	}	

运算符号的抽象类如下所示：　

	public abstract class SymbolExpression extends Expression{
	    protected Expression left;
	    protected Expression right;
	
	    //解析时应该只需要关心左右两边的终结符
	    public SymbolExpression(Expression left, Expression right){
	        this.left = left;
	        this.right = right;
	    }
	}

解析中，每个非终结符，也就是运算符只和左右两边的终结符，在这里是数字有关系，但是两个数字有可能是一个解析的结果，无论什么种类，都是Expression的实现类，于是在对运算符解析的子类中增加了一个构造器函数，传递左右两个表达式，具体的加法减法解析器如下所示：

加法解析器：

	public class AddExpression extends SymbolExpression{
	    public AddExpression(Expression left, Expression right){
	        super(left, right);
	    }
	
	    //把左右两个数字加起来
	    public int interpreter(HashMap<String, Intrger> var){
	        return super.left.interpreter(var) + super.right.interpreter(var);
	    }
	}

减法解析器：

	public class SubExpression extends SymbolExpression{
	    public SubExpression(Expression left, Expression right){
	        super(left, right);
	    }
	
	    //把左右两个数字相减
	    public int interpreter(HashMap<String, Intrger> var){
	        return super.left.interpreter(var) - super.right.interpreter(var);
	    }
	}

至此，解释器完成了，想扩展乘除你也可以看到，很简单的实现类就可以了。

##解释器之适用场景

至于应用场景么，当一个语言需要解释执行，并且你可以将该语言中的句子表示为一个抽象的语法树，可以使用该模式，而当你遇到以下情况时，使用该模式的效果最好：

* 该文法比较简单。因为对于复杂的文法，文法层次变得庞大，无法管理。
* 效率不是此问题的关键。

以上。欲知后式如何，且听下回分解。



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
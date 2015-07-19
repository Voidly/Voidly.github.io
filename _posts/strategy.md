title: 设计模式之第8章-策略模式(Java实现)
date: 2015-01-21 12:25:36
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“年前大酬宾了啊，现在理发冲500送300，冲1000送500了。鱼哥赶紧充钱啊，理发这事基本一个月一回，挺实惠的啊。不过话说那个理发店的老板好傻啊，冲1000才送500，不如冲两次500，这样可以送600呢。”“这只能说明你不是很笨，但是也算不上聪明。”“啊？难道我想错了？”“这是一种策略，策略，懂？他如果是冲1000送700的话你是不是很大的可能性冲500？而不是1000，但是如果这样的话，在“聪明人”，对，没错，就是你这样的人来说，冲两次500表示对于老板的智商上的鄙视，那么，你就会冲1000了吧，这么说来，你懂了么？”“好吧，亏我还是策略模式，这点小把式就把我给蒙骗了，丢人丢大发了。”（编者按：小子，你还是图样图森破啊。）

##策略模式之自我介绍

洒家呐，是一种比较简单的模式，定义如下：

Define a family of algorithms, encapsulate each one, and make them interchangeable。翻译过来就是说：定义一组算法，或将每个算法都封装起来，并且使他们之间可以互换。下面给出具体类图：

![](http://images.cnitblog.com/blog/666211/201501/201937010169004.png)

由于图中的介绍已经很详细了，所以就不再赘述，我使用的就是面向对象的继承和多态机制，理解起来那可是相当的容易~

##策略模式之自我分析

　若说到好处和劣势么，恩，先说劣势吧：

* 客户如果想选择一个合适的策略，那么就务必了解所有的策略以及各个策略之间有什么不同，这个时候就不得不暴漏策略具体的实现。不过嘛，魔高一尺道高一丈（好像哪里有点不对、、、不管了，继续），这个缺点可以配合着先前讲的工厂方法模式来弥补了。
* Strategy与Context之间的通信开销，由于策略算法不一定全部用得到，所以会带来一定的浪费。
* 增加了对象的数目。

优势：

* 算法可以自由切换。
* 避免使用多重判断语句，消除一些条件语句。
* 扩展性良好。
* 可以根据不同的时间/空间的要求选择不同的策略。

##策略模式之实现

话说是骡子是马拉出来遛遛，既然如此，那洒家就把代码亮出来，请各位戴好墨镜。以下代码就根据理发店充值活动来实现的。首先是抽象的策略接口：

	public interface Strategy{
	    //每个充值的方法都是一个算法
	    public void reCharge();
	}

然后是冲500送300的策略：

	public class St1 implements Strategy{
	    public viod reCharge(){
	        System.out.println("冲500送300");
	    }
	}

接着是冲1000送500的策略的具体实现：

	public class St2 implements Strategy{
	    public viod reCharge(){
	        System.out.println("冲1000送500");
	    }
	}

接下来这个还需要一个封装类用于放这些个策略，方便使用，也就是类图中的Context封装类，代码如下：

	public class Context{
	    //构造函数，你要入手哪一个优惠政策
	    private Strategy strategy;
	    public Context (Strategy strategy)
	    {
	        this.strategy = strategy;
	    }
	
	    //入手优惠政策
	    public void reCharge(){
	        this.strategy.reCharge();
	    }
	}

通过构造函数把优惠政策传进去，然后用reCharge方法来执行相关的策略方法。什么，还不懂？好吧好吧，服了你啦，看洒家写一个测试类：

	public class Y{
	
	    public static void main(String[] args) {
	        Context context;
	        //入手第一个优惠
	        context = new Context(new St1());
	        context.reCharge();
	
	        //入手第二个优惠
	        context = new Context(new St2());
	        context.reCharge();
	    }
	}　　

好了，具体实现到此结束。下面介绍一下应用的场景。

##策略模式之运用场景

当存在以下情况时，可以考虑使用策略模式：

* 许多相关的类仅仅是行为有异。
* 需要使用一个算法的不同变体。
* 一个类定义了多种行为。
* 需要屏蔽算法规则。


---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**

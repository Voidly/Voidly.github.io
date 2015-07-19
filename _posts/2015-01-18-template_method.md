---
title: 设计模式之第3章-模板方法模式(Java实现)
layout: post
date: 2015-01-18 20:45:17
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
comments: true
---
"那个，上次由于我老婆要给我做饭，所以就没有说完就走掉了、、、这个那个"。这次和以前一样，先来开场福利（工厂方法模式已被作者踹下场）。由美女抽象工厂介绍一下适用场景~大家欢迎

##抽象工厂之应用场景

* 一个系统要独立于它的产品的创建、组合和表示时。
* 一个系统要由多个产品系列中的一个来配置时。
* 当你要强调一系列相关的产品对象的设计以便进行联合使用时。
* 当你提供一个产品类库，而只想显示它们的接口而不是实现时。

“人家要讲的就这么多了，接下来还是让今天的主角-诗人模板方法哥哥了。”

“天苍苍，野茫茫，风吹雾霾见工厂~你看你夫妻俩生产那么多的吃的，把空气都污染的不行了，雾霾越来越大了。”“还不是因为作者鱼哥这个吃货”（抽象工厂碎碎念。“阿嚏，谁又想我了”。某作者说完又拿起辣条吃了起来、、、），接下来，我先来个自我介绍吧。

##模板方法之自我介绍
其实大家经常就用到我，只是不知道我叫什么罢了。对于我的定义如下：

Define the skeleton of an algorithm in an operation, deferring some steps to subclasses.Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.听抽象妹纸说你们不太懂中文，那我就给翻译下：定义一个操作中的算法的框架，而将一些步骤延迟到子类中。使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

我的通用类图如下所示：

![](http://images.cnitblog.com/blog/666211/201501/181134537299904.jpg)

其中Abstract类是抽象类，其实也就是一抽象模板，定义并实现了一个模板方法。这个模板方法一般是一个具体的方法，它给出了一个顶级的逻辑骨架，而逻辑的组成步骤在相应的抽象操作中，推迟到子类实现。

ConcreteClass实现父类所定义的一个或者多个抽象方法，每一个AbstractClass都可以有任意多个ConcreteClass与之对应，而每一个ConcreteClass都可以给出这些抽象方法的不同实现，从而使得顶级逻辑的实现各不相同。好了，基本情况就是这样的了。

##模板方法之自我分析

金无足赤，人无完人~先说不足乃我一大爱好：

* 一般情况下都喜欢在抽象类声明最抽象最一般的事物属性方法，实现类实现具体事物的属性和方法，我却反其道而行之，因此在在复杂的项目中会对代码的阅读造成一定的影响。

优点：

* 封装不变的部分，扩展可变的部分。
* 提取了公共代码，便于维护。
* 行为由父类控制，子类实现。

俗话说：Talk is cheap,show me the code。所以接下来我就实现一个简单的模板方法模式来加深一下尔等的理解。

##模板方法之具体实现

既然要实现，那就来个简单的栗子，想必你们都知道世上有两种生物，一种的具体活动就是“吃饭，睡觉，打豆豆。”（作者按：豆豆躺枪。）还有一种就是“吃饭，睡觉，敲代码。”（程序猿躺枪。）这个很简单，只需要一个动物的模型，也可能是其它物种，此处定义为动物。为了清楚认识我，先来个不是由模板方法实现的代码。首先来实现一下动物的代码：　

	public abstract class Animal{
	    //生物都需要吃东西
	    public abstract void eat();
	    //生物也都要睡觉
	    public abstract void sleep();
	    //其他活动
	    public abstract void otherthing();
	    //日常活动
	    public abstract void activities();
	}

每种动物都要有吃东西、睡觉、娱乐活动、然后是一天的总结。但是每个物种的具体实现又不同，下面先给出“吃饭睡觉打豆豆”的具体实现。

	public class AnimalY1 extends Animal{
	
	    public void eat(){
	        System.out.println("Y1动物吃糖葫芦和辣条");  //想到作者的都是出现幻觉的。
	    } 
	    public void sleep(){
	        System.out.println("Y1动物睡在绳子上");  //想到小笼包的罚你在墙角面壁思过。
	    } 
	    public void otherthing(){
	        System.out.println("打豆豆");
	    } 
	
	    public void activities(){
	        
	        this.eat();
	        this.otherthing();
	        this.sleep();
	
	    }
	}

接下来就是第二个生物，程序猿的具体实现：　

	public class AnimalY2 extends Animal{
	
	    public void eat(){
	        System.out.println("吃外卖");  //好心酸有木有
	    } 
	    public void sleep(){
	        System.out.println("睡在工位上");  //已泪目
	    } 
	    public void otherthing(){
	        System.out.println("敲代码");
	    } 
	
	    public void activities(){
	        
	        this.eat();
	        this.otherthing();
	        this.sleep();
	
	    }
	}

发现什么没有？没有？再仔细看看，是不是有一个activities每日总结都一样？对了，这个是共有的，自然要出现在抽象类中，尔等要记住一个原则：DRY。What，吾辈说错了？尔等说记得明明是DIY？可笑之极，Don't take it for granted。吾辈的意思是：Don't repeat yourself。如若在代码中发现多次重复，那么就需要想一想你的设计是不是有问题，如果不能说明原因，那就重构吧。下面我们做下修改，用上模板模式来做，首先修改一下抽象类：

	public abstract class Animal{
	    //生物都需要吃东西
	    public abstract void eat();
	    //生物也都要睡觉
	    public abstract void sleep();
	    //其他活动
	    public abstract void otherthing();
	    //日常活动
	    public void activities(){
	        this.eat();
	        this.otherthing();
	        this.sleep();
	    }
	}
在抽象的模板方法中定义了之后，把实现类的activies方法就可以移除了具体就不说了。测试类还需要否？那么简单的就不来了吧、、、好了，就到这里了

“仰天大笑出门去，我辈岂是蓬蒿人~”。“客官请留步，啊呸，模板请留步，你还没讲场景~~”。只见模板一个燕返便回来了。作者已愣住（作者碎碎念：传说中佐佐木小次郎的燕返竟然让他用于身法，这也太、、、）

##模板方法之使用场景

* 一次性实现一个算法的不变的部分，并将可变部分留给子类实现。
* 控制子类扩展。
* 各子类中公共行为可以被提取出来放到一个公共父类中。

（作者抹了抹头上的冷汗，往嘴里扔进一颗巧克力，道：还好这次没再出什么岔子~吃个巧克力压压惊），好了模板方法到此就讲完了，欲知下次讲什么？且听下回分解。



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**


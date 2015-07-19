title: 设计模式之第19章-中介者模式(Java实现)
date: 2015-01-31 01:12:32
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“测试妹纸找你，你的代码出问题了。”“美工妹纸让你看看界面怎么样。”身为程序员总要和各种人打交道，但是如果再分为前端、后端工程师的话，那么关系就会错综复杂起来了，这个时候如果有中介者进行中转，类似于星型网络拓扑的交换机，那么该有多好。（PS：注孤生啊，和测试妹纸、美工妹纸什么的一起讨论增进感情多好，那么好的机会都不珍惜。编者按：我是要做那个中介者，懂么？中介者！众人：good job!）“鱼哥，叫我干嘛？”真是说曹操曹操到，刚刚正说你来着，行了，你来说吧。

##中介者模式之自我介绍

我乃传说中的中介者，又名“Mediator”，定义如下：Define an object that encapsulates how a set of objects interact.Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.翻译为：用一个中介对象封装一系列的对象交互，中介者使各对象不需要显示地相互作用，从而使其耦合松散，而且可以独立地改变它们之间的交互。通用类图如下：

![](http://images.cnitblog.com/blog/666211/201501/301227229256825.png)

##中介者模式之自我分析

都说到这个份上了，自然也要简单的分析一下自己：

优势：

* 减少了子类的生成。这样各个Colleague类可以被重用。
* 将各个Colleague进行解耦。这样的话，就可以独立的改变和复用各种Colleague和Mediator类。
* 简化了对象协议。使得Mediator和各个Colleague一对多交互，把关系进行简化，有利于理解维护和扩展。
* 对对象如何协作进行了抽象，有助于理解一个系统中各个对象如何交互。

劣势：

* 使控制集中化，所以中介者会比较复杂，然后使得中介者变得难于维护。

##中介者模式之实现

实现嘛，既然你们要看，那我就拿一个Team的个别部分做一个Demo吧。首先我有一个抽象同事类，要和你谈谈：

	public abstract class AbstractColleague{
	    protected AbstractMediator mediator；
	    public AbstractColleague(AbstractMediator mediator){
	        this.mediator = mediator;
	    }
	}

接下来就是程序员的具体实现类了，其中有三个方法，，两个是自己执行的方法，一个是美工妹纸的任务，通过中介者交给美工妹纸做：

	public class Programmer extends AbstractColleague{
	    public Programmer(AbstractMediator mediator){
	        super(mediator);
	    }
	    //美工妹纸的任务，通过中介者交给美工
	    public void notifyifyUIer(){
	        System.out.println("告诉美工妹纸帮忙美化界面");
	        super.mediator.beautifyUI();
	    }
	
	    public void code(){
	        System.out.println("写程序，开发");
	    }
	
	    public void view(){
	        System.out.println("看看界面如何");
	    }
	}

然后是美工妹纸的具体实现类，一个是自己的工作，一个是程序员的事儿，通过中介交给程序员：

	public class UIer extends AbstractColleague{
	    public UIer(AbstractMediator mediator){
	        super(mediator);
	    }
	
	    //程序员的任务，通过中介者交给程序员
	    public void notifyProgrammer(){
	        System.out.println("让程序员看看界面");
	        super.mediator.view();
	    }
	
	    public void beautifyUI(){
	        System.out.println("美化界面");
	    }
	}

接下来就是重头戏，压轴，也就是说抽象中介者抽象类的实现：

	public abstract class AbstractMediator{
	    //程序员
	    protected Programmer programmer;
	    //UI妹纸
	    protected UIer ui;
	    //构造函数
	    //set、get方法，此处略去
	
	    //鱼哥用来处理多个对象间的关系
	    public abstract void beautifyUI();
	    public abstract void view();
	
	}　

最后是中介类的具体实现：

	public class Mediator extends AbstractMediator{
	    //鱼哥的协调策略
	    public abstract void beautifyUI(){
	        super.ui.beautifyUI();
	    }
	    public abstract void view(){
	        super.programer.view();
	    }
	}

到此，一个中介者模式就实现了。如果还有其他的相互关联的同事，如测试妹纸、市场、项目经理等等都可以加进去，由中介者进行关联。

##中介者模式之应用场景

接下来我就介绍一下在哪些情况下，可以使用我：

* 一组对象以定义良好但是复杂的方式进行通信。
* 一个对象引用其他很多对象并且直接与这些对象通信，导致难以复用该对象。
* 想定制一个分布在多个类中的行为，而又不想生成太多子类。

当你在项目中遇到以上情况或者类似的情况时，可以使用我来进行对象间的通信，这样会把复杂的问题简单化~虽然对象过多的时候，会导致中介者过于庞大。恩，鱼哥，我讲完了。（PS：给大家说这句话。）哦，大家，我讲完了。（作者满头黑线的离开了、、、）

---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
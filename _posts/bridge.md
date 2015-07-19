title: 设计模式之第10章-桥接模式(Java实现)
date: 2015-01-23 12:21:04
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“一入软件深似海，从此早睡是路人。黑夜给了我黑色的眼睛，我却用他去寻找八阿哥。”“怎么了，又来那么多的感慨啊。”“还能有什么啊，老板是说让换个APP做，这个APP现在不行了，这都是这三个月来第4个APP了、、、”“这个啊，那不是简单的很么？”“简单，别闹了，你以为APP是大白菜啊，说有就有啊。”“这个时候就是我桥接模式出场的时候了~”

##桥接模式之自我介绍

大家好，我的英文名字叫“Qiaojie Moshi”，我的中文名字叫桥接模式。（欸？哪里优点不对，哦，这小子，糊弄我呢，碎碎念一阵后作者对着桥接模式一阵拳打脚踢）。桥接模式鼻青脸肿的说“我的英文名字是、、、是Bridge Pattern”，不是先前说的那个，开个玩笑而已、、、至于下那么重的手么。“嘿嘿，那个手滑了”。相关定义如下：Decouple an abstraction from its implementation so that two can vary independently。（作者盯着桥接模式。）“老大，这次是真的，我没开玩笑。”“哦，没事，我就随便看看。”翻译过来是将抽象和实现解耦，使得两者可以独立地变化。我的重点就是“解耦”，如何让抽象和实现解耦就是需要了解的重点。下面先来看看桥接模式的类图：

![](http://images.cnitblog.com/blog/666211/201501/222126083759931.png)

现在来详细介绍下各个角色：

* Abstract：抽象化角色。主要职责是定义出该角色的行为，同时保存一个对实现化角色的引用，该角色一般是抽象类。
* Implementor：实现化角色。是接口或者抽象类，定义角色必需的行为和属性。
* RefinedAbstract：修正抽象化角色。实现接口或抽象类定义的方法和属性。
* ConcreteImplementor*：具体实现方法类。

##桥接模式之自我分析

身为一个“每日三省吾身之人”，缺点早已改进，因此剩下的也就只有优点了（作者按：别嘚瑟了，赶紧说重点。）别扔鸡蛋啊，咳咳，那个，我的优点有下面这几条：

* 分离接口及其实现部分。这也是我的主要特点，毕竟也是为了解决继承的缺点而提出的设计模式嘛。
* 优秀的扩充能力。
* 实现细节对客户的透明。因此你们完全不用关心细节的实现，它已经由抽象层通过聚合关系完成了封装。

 ##桥接模式之实现

既然你们不信，看来今天不露两手是镇不住你们了，好了，看我的大招：

既然说到了APP，那我们就来谈谈APP，首先是抽象APP类：

	public abstract class App{
	    //首先要设计，什么数据库设计、框架选取、概要设计、详细设计等等
	    public abstract void beDesigned();
	    //然后是编码实现
	    public abstract void beCoded();
	    //最后是测试
	    public abstract void beTest();
	}

假设是社交类APP：

	public class ChatApp extends App{
	    //首先要设计，什么数据库设计、框架选取、概要设计、详细设计等等
	    public abstract void beDesigned(){
	        System.out.println("调研，写文档、写文档、、、");
	    }
	    //然后是编码实现
	    public abstract void beCoded(){
	        System.out.println("我是一个程序员，加班Coding中");
	    }
	    //最后是测试
	    public abstract void beTest(){
	        System.out.println("test、test、test、、、");
	    }
	}

然后APP产品自然是由互联网公司生产的了，下面是抽象的公司类：

	public abstract class Corp{
	    //定义一个抽象APP对象，但是不知道具体是什么
	    private App app;
	    //构造函数，由子类定义传递具体产品
	    public Corp(App app){
	        this.app = app;
	    }
	
	    //制作APP
	    public void developApp(){
	        this.app.beDesigned();
	        this.app.beCoded();
	        this.app.beTest();
	    }
	}

这里是有参构造，因为继承的子类需要重写自己的有参构造函数，把APP的类传进来，首先看一下APP公司的实现：

	public class AppCorp extends Corp{
	    //生产的App 待定，据需求而定
	    public AppCorp(App app){
	        super(app);
	    }
	
	    //生产App
	    public void developApp(){
	        super.developApp();
	        System.out.println("要什么制作什么APP");
	    }
	}	

具体使用看如下的场景类：

	public class Client{
	    public static void main(String[] args) {
	        AppCorp appCorp = new AppCorp(new ChatApp());
	        appCorp.developApp();
	    }
	}

怎么样，是不是很厉害，是不是对我的敬仰犹如滔滔江水奔流不止啊~（作者按：滚。众读者已吐得天昏地暗。）

##桥接模式之应用场景

刚刚桥接模式有事走了（别拉我，让我讲完啊~~~），所以由我给大家继续讲解应用场景：

* 当你不希望抽象和实现之间有一个固定的绑定关系的时候。
* 类的抽象和它的实现都应该可以通过生成子类的方法加以扩充。
* 对一个抽象的实现部分的修改应对客户不产生影响。
* 你想对客户完全隐藏抽象的实现部分。
* 有许多的类要生成。
* 你想对多个对象间共享实现，但同时要求客户并不知道这一点。

以上就是本次要说的全部内容，荆轲刺秦王，设计模式心中藏~（好像有什么不对）



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
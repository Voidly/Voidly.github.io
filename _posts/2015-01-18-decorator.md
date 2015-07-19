---
title: 设计模式之第4章-装饰模式(Java实现)
layout: post
date: 2015-01-18 20:56:04
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
comments: true
---
“怎么了，鱼哥？”

“唉，别提了，网购了一件衣服，结果发现和商家描述的差太多了，有色差就算了，质量还不好，质量不好就算了，竟然大小也不行，说好的3个X，邮的却是一个X的，不说了，退货去。你先开讲吧，你说说就一个东西还装饰个什么劲儿。”（装饰模式石化中：这关我什么事儿撒。）恩，今天由我来讲，讲之前先来个段子：话说面条被追到一个理发店，出来一个方便面，然后追他的人一把抓住他就开打：小子（第四声），烫个头发我就不认识你了么？其实那人认错了，出来的真的是方便面，我认识的，因为方便面屁股上有胎记，恩。然后为什么会认错呢？没错，就是他认为面条“装饰了一下”。好了，我就是装饰。不过鱼哥的衣服真的不怨我、、、

##装饰模式之自我介绍

先来看下有关我的定义:Attach additional responsibilities to an object dynamically keeping the same interface. Decorators provide a flexible alternative to subclassing for extending functionality.翻译过来的意思就是动态的给一个对象添加一些额外的职责。就增加功能来说，装饰模式想比生成子类更为灵活。下面的是我的类图：

![](http://images.cnitblog.com/blog/666211/201501/181404234179260.jpg)

Component是定义一个对象接口，可以给这些对象动态的添加职责，ConcreteComponent是定义了一个具体的对象，也可以给这个对象添加一些职责。Decorator，装饰抽象类，继承了Component从外类来扩展Component类的功能，但对于Component来说，是无需知道Decorator的存在，至于ConcreteDecorator就是具体的装饰对象，起到给Component添加职责的功能。（装饰模式：容我抽根烟，喝杯水）

讲到哪里来着？哦，想起来了。接下来我就谈谈Advantages和Disadvantages吧。

##装饰模式之自我分析

我嘛，比较均衡，何为均衡呢，就是优点和缺点一半一半，主要优点两个，如下：

* 比静态继承更为灵活。比如说可以用添加和分离的方法，用装饰在运行时刻增加和删除职责。
* 可以避免在层次结构高层的类有太多的特征。

主要缺点：

* Decorator与它的Component不一样。Decorator是一个透明的包装，如果从对象标识观点出发，一个被装饰的组件与此组件有差别。因此不能依赖对象标识。
* 使用装饰模式会产生很多小的对象。这对于它来说很难进行订制。而且排错也很困难。

##装饰模式之实现

光说不练假把式，既然如此，我就给你们露一手，这次就拿面条烫发来当栗子，首先是面条的抽象类：

	public abstract class Noodles{
	    //抽象方法
	    public abstract void say();
	}

抽象类比较简单，只有一say的属性，接下来是具体的面条的类的实现：

	public  class sayNoodles extends Noodles{
	    //抽象方法
	    @Override
	    public void say(){
	        System.out.println("我是面条");
	    }
	}

他主要就是实现了抽象类的方法，描述自己是面条，不是其它的东西。接下来的就比较有意思了，就是这次的重点抽象类，装饰，一般都是把装饰类作为一个抽象类，因为要装饰的东西不仅仅只有一种， 可能装饰很多东西，比如说一个面条可以先烫发，在染发，然后焗一下等等，写到一个实现类的话就太臃肿，不利于扩展等，所以最好用一个抽象类，然后具体实现不同的装饰再具体子类实现，下面就是一个装饰的抽象类：　

	public abstract class Decorator extends Noodels{
	    private Noodels noodles = null;
	    //通过构造函数传递被修饰的东西
	    public Decorator(Noodels nood)
	    {
	        this.noodles = nood;
	    }
	
	    //委托给被修饰着执行
	    @Override
	    public void say(){
	        this.noodles.say();
	    }
	
	}

接下来就是一个实现给面条烫发，大变方便面的装饰类了：

	public class ConcreteDecorator extends Decorator{
	    //定义被修饰者
	    public ConcreteDecorator(Noodles nood){
	        super(nood);
	    }
	
	    //定义自己的修饰方法
	
	    private void dsay(){
	        System.out.println("我烫了头发");
	    }
	
	    //重写父类方法
	    public void say(){
	        this.dsay();
	        super.say();
	    }
	
	}

接下来可以通过测试类测试一下装饰后的效果：

	public class Test{
	    public static void main(String[] args) {
	        Noodels noodels = new sayNoodles();
	        //进行装饰，烫发开始
	        noodels = new ConcreteDecorator(noodels);
	        noodels.say();
	    }
	}	

好了，有关装饰模式的到此就没了。欲知后式（此为模式）如何，且听下回分解~（作者按：卧槽，竟然抢我台词，别理我，我想静静，别问我静静是谁）

##装饰模式之应用场景

* 不影响其他对象的情况下，以动态、透明的方式给单个对象增加职责。
* 处理可以撤销的职责。

以上。欲知后式何如？且听下回分解。




---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
---
title: 设计模式之第12章-享元模式(Java实现)
layout: post
date: 2015-01-25 11:00:20
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“怎么回事，竟然出现了OutOfMemory的错误。鱼哥，来帮我看看啊。”“有跟踪错误原因么？是内存泄露么？”“不是内存泄露啊，具体原因不知道啊。对了，有说新对象申请不到内存空间。”“这个原因么，我曾写过一篇博文：叫OutOfMemory简单分析。不过你的明显是因为代码问题，产生对象太多，导致内存被耗尽，正好一会有堂课，讲的正好能解决你的问题。”（嘿嘿，轮到我享元模式出场了~）

##享元模式之自我介绍

我，享元模式乃是池技术中的重要实现方式，具体定义如下：Use sharing to support large numbers of fine-grained objects efficiently.翻译过来就是说使用共享对象可以有效的支持大量的细粒度的对象。由定义可知两个要求：细粒度的对象和共享对象。前面那个人出现的问题就是内存溢出，就是因为分配了太多的对象导致了内存溢出，当然还会有损程序的性能。那么如何解决呢，没错就是我所提供的共享技术。在这之前先了解一下对象的外部状态以及内部状态。

细粒度对象不可避免会使对象数量多且性质相近，那我们就将这些对象分为两部分，也就是外部状态和内部状态。下面解释一下内部状态与外部状态：

* 外部状态：就是对象得以依赖的一个标记，随环境的改变而改变，不可以共享的状态，比如说用户名以及ID之类的信息。
* 内部状态：就是对象可以共享出来的信息，存储在享元对象内部并且不会随环境的改变而改变，不必存储在具体某个对象中，属于可以共享的部分。

我的类图如下：

![](http://images.cnitblog.com/blog/666211/201501/242143533919140.jpg)

类图中每个类行驶的功能以及详细的介绍了，我也就不废话了。

##享元模式之自我分析

优点：

* 减少应用程序创建的对象。
* 降低程序内存占用。
* 增强程序性能。

　　缺点：

* 提高了系统复杂性。
* 需要分离出外部状态和内部状态。

##享元模式之实现

俗话说，没有银弹，但是却有通用的实现的代码，我水平那么高，自然会用通用的代码来实现我的模式~我的模式我做主，首先是抽象享元角色：

	public abstract class Flyweight{
	    //内部状态
	    private String intrinsic;
	    //外部状态
	    protected final String Extrinsic;
	    //要求享元角色必须接受外部状态
	    public Flyweight(String extr){
	        this.Extrinsic = extr;
	    }
	    //定义业务操作
	    public abstract void operate();
	
	    //内部状态的getter/setter
	    public String getIntrinsic(){
	        return intrinsic;
	    }
	
	    public void setIntrinsic(String intrinsic)
	    {
	        this.intrinsic = intrinsic;
	    }
	}

抽象享元角色一般为抽象类，在实际项目中，一般是一个实现类，它是描述一类事物的方法，在抽象角色中，一般需要把外部状态和内部状态定义出来，避免子类随意扩展，下面是具体的享元角色实现的代码：

	public class ConcreteFlyweight extends Flyweight{
	    //接受外部状态
	    public ConcreteFlyweight(String extri){
	        super(extri);
	    }
	    //根据外部状态进行逻辑处理
	    public void operate(){
	        //业务逻辑
	    }
	}

这就是实现自己的业务逻辑，然后接收外部的状态，以便内部业务逻辑对外部状态的依赖，在抽象享元中加入final关键字也是有原因的，为了防止无意见修改导致池的混乱。

下面是享元工厂：

	public FlyweightFactory{
	    //定义容器
	    private static HashMap<String, Flyweight> pool = new HashMap<>();
	    //享元工厂
	    public static Flyweight getFlyweight(String extri){
	        //需要返回的对象
	        Flyweight flyweight = null;
	        //判断是否在池中有该对象
	        if(pool.containskey(extri)){
	            flyweight = pool.get(extri);
	        }
	        else{
	            //根据外部状态创建享元对象
	            flyweight = new ConcreteFlyweight(extri);
	            pool.put(extri, flyweight);
	        }
	        return flyweight;
	    }
	}

通用代码就这么多，可以根据具体需要往里面套~具体的我就不多说了。

##享元模式之使用场景

当遇到以下情况时，就使用我吧，效果那是杠杠的：

* 一个应用程序使用了大量的对象。
* 完全由于使用大量对象造成很大的存储开销。
* 对象的大多状态都是外部状态。
* 如果删除对象的外部状态，那么可以用相对较少的共享对象取代很多组对象。
* 应用程序不依赖于对象的标识。

以上。今天就到此为止。荆轲刺秦王，设计模式使我强~



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.gitcafe.com/2015/01/25/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E7%AC%AC12%E7%AB%A0-%E4%BA%AB%E5%85%83%E6%A8%A1%E5%BC%8F-Java%E5%AE%9E%E7%8E%B0/)。**
> **博文链接：<http://voidy.net/>**
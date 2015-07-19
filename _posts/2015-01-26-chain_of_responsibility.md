---
title: 设计模式之第13章-职责链模式(Java实现)
layout: post
date: 2015-01-26 23:13:38
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“请假都那么麻烦，至于么。”“咋的了？”“这不快过年了么，所以我想早两天回去，准备一下，买买东西什么的，然后去给项目经理请假，但是他说快过年了，所以这个事儿他没法决定，所以只能找总经理了，你说麻不麻烦。”“这不是很正常么，现在好多事不都是这样的，尤其是那些大公司，制度完善，分工更加细致，层级多，更麻烦。不过这就牵扯到今天的职责链模式了。”“什么？这都能扯到传说中的职责链模式？”

##职责链模式之自我介绍

当当当当~我就是人见人耐，花见花开，车见车爆胎的职责链是也。人们都我的定义是：Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request.Chain the receiving objects and pass the request along the chain until an object handles it.意思是：使多个对象都有机会处理请求，从而避免了请求的发送者和接受者之间的耦合关系，将这些对象连成一条链，并沿着这条链传递该请求，直到有对象处理它为止。职责链重点在“链”上，由一条链去处理相似的请求在链中决定谁来处理这个请求，并返回相应的结果，它的通用类图是： 

![](http://images.cnitblog.com/blog/666211/201501/250957102975177.png)

至于类图的含义么，讲的这么清楚的图我就不再赘述了。（作者按：挺好，颇有我的风格。众人：被作者带坏的某人。）

##职责链模式之自我分析

我呢，比他们差远了，因为印象中我好像全是缺点的说。（场下响起热烈的掌声。众人：看看，人家多谦虚，哪像前面几个，都声称自己毫无缺点，谁信啊，哪有那么完美的存在，俗话说:不存在银弹的好吗。躺枪的某些模式们。）我的缺点嘛，如下：

* 性能不是很好，由于从链头开始遍历，因此链比较长的时候性能问题将会暴漏。
* 调试不便。链长后所带来的问题。
* 不保证被接受。若无明确的接受者，无法保证一定被处理。

突然想起来貌似还是有那么一丢丢优点的：

* 降低耦合度。
* 增强了给对象指派职责的灵活性。

##职责链之实现

我们就以大家常见的请假来具体实现一下职责链模式。首先是员工，也就是程序员的接口：

	public interface IEmployee{
	    //获取请求级别
	    public int getType();
	    //获取个人请示
	    public String getRequest();
	}

接下来是员工的具体实现类：

	public class Employee implements IEmployee{
	    //请假
	    private String request = "";
	    //请求级别
	    private int type = 0;
	
	    public Employee(String req, int type){
	        this.req = request;
	        this.type = type;
	    }
	
	    public int getType(){
	        return this.type;
	    }
	
	    public String getRequest(){
	        return this.request;
	    }
	}

员工请假总要找人请示，而管理层的人的接口Handler代码如下：

	public abstract class Handler{
	    public final static int MANAGER_REQUEST = 1;
	    public final static int PROJECT_MANAGER = 2；
	
	    //能处理的级别
	    private int level = 0;
	    //责任传递给下一个负责人
	    private Handler nextHandler;
	    //每个类说明一下自己能处理的请求
	    public Handler(int lev)
	    {
	        this.lev = level;
	    }
	    //处理请求
	    public final void HandleMessage(IEmployee emp){
	        if (emp.getType() == this.level) {
	            this.response(emp);
	        }
	        else{
	            if(this.nextHandler != null){
	                //处理不了请求，将请求上报给上层
	                this.nextHandler.HandleMessage(emp);
	            }
	            else{
	                //无上层领导了
	                System.out.println("默认好不好了");
	            }
	        }
	    }
	    //不归你管的请求，不能僭越，要找下一层
	    public void setNext(Handler han){
	        this.nextHandler = han;
	    }
	
	    //对于请求的回应
	    protected abstract void response(IEmployee emp);
	}

下面是具有管理权限的第一层项目经理的实现类，实现类中首先确定自己能处理的级别，然后就是对于请求作出回应。

	public class ProjectManager extends Handler{
	    //只能处理部分员工的请求：
	    public ProjectManager(){
	        super(Handler.PROJECT_MANAGER)；
	    }
	    //答复
	    protected void response(IEmployee emp){
	        System.out.println("员工要请假");
	        System.out.println(emp.getRequest());
	        System.out.println("我管不了啊，要问领导了");
	    }
	}

接下来是总经理类，同样是确定能处理事情的级别以及作出回应。

	public class Manager extends Handler{
	    //请求级别
	    public Manager(){
	        super(Handler.MANAGER_REQUEST);
	    }
	    //回复
	    protected void response(IEmployee emp){
	        System.out.println("员工要请假");
	        System.out.println(emp.getRequest());
	        System.out.println("准！");
	    }
	}

好了，以上就是其具体的实现了。

##职责链模式之应用场景

　应用场景还是蛮多的，以下情况就可以用到：

* 有多个对象可以处理一个请求的时候。
* 可处理一个请求的对象集合应被动态的指定。
* 在不明确指定接收者的情况下，向多个对象中的一个提交一个请求。

好了。That‘s all。下面由鱼哥讲话。（我勒个去，这孩子好有眼力劲儿。）那个，我也没什么可说的了。吃饭去了。

---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
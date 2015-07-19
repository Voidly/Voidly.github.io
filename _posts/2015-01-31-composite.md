---
title: 设计模式之第22章-组合模式(Java实现)
layout: post
date: 2015-01-31 14:36:31
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
comments: true
---

“鱼哥，有没有什么模式是用来处理树形的“部分与整体”的层次结构的啊。”“当然”“没有？”“有啊。别急，一会人就到了。”

##组合模式之自我介绍

“请问你是？怎么什么都不说就直接上来了。”“本式行不更名坐不改姓，就是组合模式来着，此次受作者之邀来讲讲我的前世今生来着。”“哦，你就是组合模式啊，久仰久仰。”“失敬失敬。”恩，首先我先说下定义：Compose objects into tree structure to represent part-whole hierarchies.Composite lets clients treat individual objects and compositions of objects uniformly.就是说将对象组合成树形结构以表示“部分-整体”的层次结构，使得用户对单个对象和组合的使用具有一致性。我的通用类图如下所示：

![](http://images.cnitblog.com/blog/666211/201501/311327156285570.png)

用户使用Component类接口与组合结构中的对象进行交互。如果接收者是一个叶节点，则直接处理请求，如果接收者是Composite，它通常将请求发送给它的子部件，在转发请求之前与/或之后可能执行一些辅助操作。

##组合模式之自我分析

　优点：

* 定义了包含基本对象和组合对象的类的层次结构。基本对象可以被组合成更复杂的组合对象，这个组合又会被组合，不断递归下去。
* 简化客户代码。客户可以一致的使用组合结构和单个对象。
* 使得更容易增加新增类型的组件。　　

缺点：

* 很难限制组合中的组件。有时希望一个组合只能有某些特定组件。使用Composite时，不能依赖类型系统施加这些约束，而必须要在运行时刻进行检查。

##组合模式之实现

实现这么简单的事还需要举栗子么？（PS：别废话，这个东西是赖不掉的，想当年工厂方法和抽象工厂夫妻俩就没赖掉。）那个好吧，我就举个栗子以便于理解：

首先是抽象的Component类：

	public abstract class Component{
	    //部分与整体共享的方法
	    public void doSth(){
	        
	    }
	}

接下来是继承自树枝的类的实现，里面包含增加、删除以及获得分支下所有的叶子或分支组件的方法，代码如下：

	public class Composite extends Component{
	    //Component容器
	    private ArrayList<Component> compList = new ArrayList<>();
	    //增加叶子/树枝节点
	    public void add (Component component){
	        this.compList.add(component);
	    }
	
	    //删除叶子/树枝节点
	    public void remove(Component component){
	        this.compList.remove(component);
	    }
	
	    //获得分支下所有的叶子/分支Component
	    public ArrayList<Component> getChildren(){
	        return this.compList;
	    }
	}

叶子节点的实现方法：

	public class Leaf extends Component{
	    
	}

组合模式就这么简单的实现了。为表诚意，我再实现个场景类来说明如何调用这些个方法：

	public class Client{
	    public static void main(String[] args) {
	        //创建根节点
	        Composite root = new Composite();
	        root.doSth();
	        //创建一个树枝
	        Composite branch = new Composite();
	        //创造叶子
	        Leaf leaf = new Leaf();
	        //组合
	        root.add(branch);
	        branch.add(leaf);
	    }
	}

此类中，显示创建根节点，然后是树枝，最后是叶子，然后这些被组合成树。就此结束。

##组合模式之应用场景


* 表示整体-部分的层次结构。
* 希望用户忽略组合对象与单个对象的不同，用户将统一的使用组合结构中的所有对象。

好了，设计模式之23式至此完结。撒花，撤走~撒有那拉，米娜。



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
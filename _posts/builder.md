title: 设计模式之第11章-建造者模式(Java实现)
date: 2015-01-24 11:50:41
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“那个餐厅我也是醉了、、、”“怎么了？”“上菜顺序啊，竟然先上甜品，然后是冷饮，再然后才是菜什么的，无语死了。”“这个顺序也有人这么点的啊。不过很少就是了，正常来说如果是中餐的话，都是先凉菜再热菜，然后是汤，最后是一些甜品什么的。西餐呐，先有头盘，用来开胃的，然后是汤（感觉好怪的说），再然后是副菜、主菜、蔬菜类、甜品、饮料来着。然后法国嘛就是blablabla、、、”（作者已被众人拖走。“让我说完啊，就剩几个国家了~啊~~”）。咳咳，题归正转。你问我是谁？且看下面的自我介绍。

##建造者模式之自我介绍

既然你诚心诚意的问了，那我就大发慈悲的告诉你，为了防止软件界被破坏，为了维护软件界的和平，建造者（PS：在GoF的中文译本里被翻译为生成器模式，当然为了 向我的启蒙书《大话设计模式》致敬，在此沿用里面的翻译，创建者~），我是穿梭在银河系的设计模式，白洞、白色的明天在等待着我，就是酱紫，瞄~我的英文定义是：Separate the construction of a complex object from its representation so that the same construction process can create different representations.我的中文定义是：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。其类图如下：

![](http://images.cnitblog.com/blog/666211/201501/241014303139320.jpg)

基本情况就是这个样子了。每个类图所行驶的功能职责图上已经详细给出。

##建造者模式之自我分析

该进行自我解剖了。哦不，是自我分析，鉴于前几个说自己没有缺点的模式的悲惨下场，我就不说没有缺点了，不过先说下优点：

* 将构造代码和表示代码分开。
* 可以对构造过程进行更精细的控制。
* 可以改变一个产品的内部表示。这个主要是因为Builder对象提供给指挥器一个构造产品的抽象接口。该接口可以使生成器隐藏这个产品的表示和内部结构。

##建造模式之实现

刚刚把鱼哥给拖走了，为了表示歉意，在此就送身为吃货的鱼哥一堆吃的聊表歉意（某鱼：阿嚏，谁又想我了）。恩，就以上菜顺序来说吧。首先是FoodBuilder，用于将上菜顺序组装好。

首先是上菜顺序的抽象类：

	public abstract class FoodSeq{
	    //各个基本方法的执行顺序
	    private ArrayList<String> seq = new ArrayList<>();
	
	    //上菜
	    protected abstract void food();
	    //上汤
	    protected abstract void soup();
	    //上酒
	    protected abstract void wine();
	
	    final public void eat(){
	        //循环，哪个先  吃哪个
	        for ( int i =0; i<this.seq.size(); i++ ) {
	            String s = this.seq.get(i);
	            if (s.equalsIgnoreCase("food")) {
	                this.food();
	            }
	            else if (s.equalsIgnoreCase("soup")) {
	                this.soup();
	            }
	            else if (s.equalsIgnoreCase("wine")) {
	                this.wine();
	            }
	        }
	    }
	
	    //获取顺序值
	    final public void setSequence(ArrayList<String> seq){
	        this.seq =seq;
	    }
	}

其次是中国菜的上菜顺序代码：

	public class ChSeq extends FoodSeq{
	    //上菜
	    protected abstract void food(){
	        System.out.println("翠花，上酸菜，哦不，上菜~");
	    }
	    //上汤
	    protected abstract void soup(){
	        System.out.println("小二，上汤了");
	    }
	    //上酒
	    protected abstract void wine(){
	        System.out.println("老板，来瓶唐宋元明清年间的陈酿");
	    }
	}

下面是FoodBuilder的抽象类：

	public abstract class FoodBuilder{
	    //建造一个模型,给一个上菜顺序
	    public abstract void setSequence(ArrayList<String> sequence);
	    //设置完毕后可以直接得到上菜的顺序图
	    public abstract FoodSeq getFoodSeq();
	}

下面是中国菜系的Builder类实现类：

	public class ChBuilder extends FoodBuilder{
	    private ChSeq ch = new ChSeq();
	    public ChSeq getChSeq(){
	        return this.ch;
	    }
	    public void setSequence(ArrayList<String> sequence){
	        this.ch.setSequence(sequence);
	    }
	}

接下来就是我们最最重要的指挥官了，他可是运筹于帷幄之中，决胜于千里之外的顶级指挥官，指挥官中的战斗机，哦嘴滑了，战斗官：

	public class Director{
	    private ArrayList<String> seq = new ArrayList();
	    private chBuilder ch = new chBuilder();
	
	    //顺序A，先上汤，后上菜，最后上酒
	    public ChSeq getASeq(){
	        //清除缓存
	        this.seq.clear();
	        //顺序A
	        this.seq.add("soup");
	        this.seq.add("food");
	        this.seq.add("wine");
	        this.ch.setSequence(this.seq);
	        return (ChSeq)this.ch.getChSeq();
	    }
	
	        //顺序B，先上菜，再上酒，最后上汤
	    public ChSeq getASeq(){
	        //清除缓存
	        this.seq.clear();
	        //顺序A
	        this.seq.add("food");
	        this.seq.add("wine");
	        this.seq.add("soup");
	        this.ch.setSequence(this.seq);
	        return (ChSeq)this.ch.getChSeq();
	    }
	}

好了，具体实现到此就完了。

##建造者模式之应用场景


* 有相同的方法但是执行顺序不同，产生不同的事件结构。
* 多个部件或零件，都可以装配到一个对象中。
* 产品类非常复杂或者产品类中的调用顺序不同的效能。

预知后事如何，且听下回分解，饿的不行了，吃饭去~



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**

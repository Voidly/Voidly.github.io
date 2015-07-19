---
title: 设计模式之第2章-抽象工厂模式(Java实现)
layout: post
date: 2015-01-16 22:24:37
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---
“上次是我的不对，贿赂作者让我先讲来着，不过老婆大人大人有大量，不与我计较，这次还让我先把上次未讲完的应用场景部分给补充上去，有妻如此，夫复何求。”（说完，摸了摸跪的发疼的膝盖，咳咳，我发四我没笑！真的！）。

##福利：工厂方法模式之应用场景

各位好，说起来应用场景，那简直是项目处处有工厂方法啊~虽然这么说有点大言不惭，但是真的是使用率超级高的说。

* 当一个类不知道它所必须创建的对象的类的时候。
* 当一个类希望由它的子类来指定它所创建的对象的时候。
* 当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时

好了，我要讲的就这么多，接下来的时间就交给我老婆了。（掌声雷动~）

##抽象工厂模式之自我介绍

大家好，今天由我来带领大家领略一下抽象工厂模式，恩，其实人家也是一种比较常用的模式啦，有关我的定义是：Provide an interface for creating families of related or dependent objecets without specifying their concrete classes.“老公，给翻译一下呗。“啊？哦，好的。”（作者按：秀恩爱，死的。。。喂，我还没说完呐。被毫无尊严拖走的作者）。这句的意思是为创建一组相关或者相互依赖的对象提供一个接口，而且无须指定它们的具体类。

抽象工厂模式的通用类图如下所示：

![](http://images.cnitblog.com/blog/666211/201501/162032060113074.png)

我呐，其实就是我老公的升级版了，在有多个业务品种、业务分类的时候，就轮到我登场了~好了，基本情况就是酱紫了。我要回去做饭了。“哎，老婆，别走，这样不行的，你还要讲解一下具体的实现，要不然他们不懂的。“可是，可是臣妾做不到啊。“很简单的，你只要这样，这样，然后这样，就可以了。“我读书少，老公你不要骗我。不然，哼哼~”

##抽象工厂模式之具体实现

上次鱼哥说做的东西品种太少，吃多了就吃腻了，所以，这次就多来几个品种，对了，上次老公的好多东西没用完，看看能不能尽量的废物利用，勤俭持家的传统美德还是要保持的~

	public interface Food{
	    public void createFood();
	
	    //每种吃的都有品牌与种类
	    public void getBrand();
	}

食物有两种抽象类，一种是糖葫芦，一种是辣条。下面的代码分别是糖葫芦和辣条的实现类：

	public abstract class AbstractTanghulu implements Food{
	    public void createFood(){
	        System.out.println("我是好吃的糖葫芦~");
	    }
	}


	public abstract class AbstractLatiao implements Food{
	    public void createFood(){
	        System.out.println("我是好吃的辣条~");
	    }
	}

每个抽象类还有两个实现类，分别是实现公共的细节具体的事情，也就是那些个食物的牌子，下面我就以糖葫芦为例进行实现，辣条的也就是类似了：

	public class OldPekingTanghulu extends AbstractTanghulu{
	    public void getBrand(){
	        System.out.println("老北京糖葫芦~");
	    }
	}

这个是糖葫芦的具体实现类，显示了它的牌子是老北京的，最有名的糖葫芦~之一了。

	public class TianjinTanghulu extends AbstractTanghulu{
	    public void getBrand(){
	        System.out.println("天津糖葫芦~");
	    }
	}

这个呢，自然就是天津的糖葫芦了，天津也算是一个好地方啊，好玩的好听的天津相声~还有鱼哥，也是我们的最爱，天津麻花~好了，有关食物的类已经实现了，接下来就是制造糖葫芦以及辣条的“工厂”的具体实现了首先就是生产糖葫芦和辣条的接口了：

	public interface FoodFactory{
	
	    //生产糖葫芦
	    public Food createTanghulu();
	
	    //生产辣条
	    public Food createLatiao();
	}

接下来能否生产真正的糖葫芦以及辣条就看这一步了，首先是生产老北京糖葫芦的实现类：

	public class OldPekingFactory implements FoodFactory{
	    //生产老北京糖葫芦
	    public Food createOldPeking();
	
	}

接下来就是生产天津糖葫芦的实现类（由于比较懒，所以就只实现了糖葫芦的，至于辣条的，你们这么聪明，自然会写的了）：

	public class OldPekingFactory implements FoodFactory{
	    //生产天津糖葫芦
	    public Food createTianjin();
	
	}

到此就完成了实现，想不想来尝尝两种糖葫芦有什么不同以及哪个比较好吃吗？不用说也知道你们想的，所以就来个测试类来做点糖葫芦来吃，不然作者又要吐槽了，下面的就是测试类，用于生产两种糖葫芦的：

	public class Y{
	    public static void main(String[] args) {
	
	        //生产老北京牌的糖葫芦
	        FoodFactory peckingFactory = new OldPekingFactory();
	
	        //生产天津牌的糖葫芦
	        FoodFactory tianjinFactory = new TianjinFactory();
	
	        Food pekingFood = peckingFactory.createTanghulu();
	        Food tianjinFood = tianjinFactory.createTanghulu();
	
	        System.out.println("生产老北京牌的糖葫芦");
	        pekingFood.createFood();
	        pekingFood.getBrand();
	
	        System.out.println("生产天津牌的糖葫芦");
	        tianjinFood.createFood();
	        tianjinFood.getBrand();
	    }
	}

“老婆，赶紧的，我饿了。“来了来了，走回去给你做吃的。”这对小夫妻，又秀恩爱~（PS：作者慢悠悠的从后面走来了）。好了，这次的就到这里了。“哎，好像缺点什么来着，缺点什么呢？”（抽象工厂模式：哎呀，老公，我忘了说应用场景了啊~~~工厂方法模式：有么？没有把。先好好做饭吧，饿死了啊、、、）欲知后事如何，且听下回分解。


> 第一篇：[设计模式之序章-UML类图那点事儿](http://voidy.gitcafe.com/2015/01/14/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%BA%8F%E7%AB%A0-UML%E7%B1%BB%E5%9B%BE%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF/)

> 第二篇：[设计模式之第0章-单例模式(Java实现)](http://voidy.gitcafe.com/2015/01/15/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E7%AC%AC0%E7%AB%A0-%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F/)

> 第三篇: [设计模式之第1章-工厂方法模式(Java实现)](http://voidy.gitcafe.com/2015/01/16/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E7%AC%AC1%E7%AB%A0-%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F-Java%E5%AE%9E%E7%8E%B0/)

参考书籍：GoF的《Design Patterns:Elements of Reusable Object-Oriented Software》

　　　　　秦小波的《The Zen of Design Patterns second Edition》



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**